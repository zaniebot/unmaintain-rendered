```yaml
number: 949
title: Add an API for lazily refreshing cache entries
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/refresh
created_at: 2024-01-17T15:12:41Z
updated_at: 2024-01-23T03:25:38Z
url: https://github.com/astral-sh/uv/pull/949
synced_at: 2026-01-10T15:39:02Z
```

# Add an API for lazily refreshing cache entries

---

_Pull request opened by @charliermarsh on 2024-01-17 15:12_

## Summary

Today, we support `--reinstall` and `--reinstall-package [package]` as arguments to `pip sync`. This flag looks at the lockfile, and does two things:

1. Purges the cache, for each package in the lockfile (or each package identified by `--reinstall-package`).
2. Marks the package for reinstallation (so, uninstalls it, then rebuilds and reinstalls it).

This is useful both for ignoring cached data and forcing a reinstall into the virtual environment.

I want to split this behavior in two, since ignoring cached data is useful in _other_ commands too, like `pip compile`, where the second part ("reinstall") isn't relevant (see: #945).

The problem with a `--refresh` flag is that we can't know the set of packages we might _try_ to read upfront. So the strategy we take for `pip sync` (where we have a lockfile) doesn't work for `pip compile` (where we might end up looking up data for transitive dependencies).

So, instead, here's a different idea: we add an API to the `Cache` to lazily refresh entries. `Cache` now accepts a `Refresh` policy, and has a `fresh_entry` method. If you call `fresh_entry`, and a `Refresh` policy is set, then we check if the `CacheEntry` has been refreshed yet; if not, we purge it from the cache.

This has the effect of (1) only purging entries we try to read, while (2) ensuring that we only purge _once_ within a single command (so, if we purge, then try to read the same wheel twice in different operations within a single process, we _do_ use the newly-cached data).

The downside here is that we need to be careful about when we call `.fresh_entry` vs `.entry`, _and_ `.fresh_entry` introduces `async` into more places (since we need to use our `once_map` to ensure we wait on purging to complete across concurrent tasks).


---

_Converted to draft by @charliermarsh on 2024-01-17 15:12_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-01-17 15:12_

---

_Review requested from @konstin by @charliermarsh on 2024-01-17 15:12_

---

_Comment by @charliermarsh on 2024-01-17 15:13_

Unfinished work includes:

- Adding `Refresh` support to the install plan. (There are some stopships around this, but I understand how to solve it.)
- Wiring this up to the CLI.

---

_@charliermarsh reviewed on 2024-01-17 15:15_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/lib.rs`:624 on 2024-01-17 15:15_

Needs `Arc` for `Clone`, because `RegistryClient` is `Clone`, and `RegistryClient` has `Cache`.

---

_@charliermarsh reviewed on 2024-01-17 15:17_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/lib.rs`:218 on 2024-01-17 15:17_

So the `refresh` mechanism works by purging from the cache. This does mean that if you run with `--refresh`, then your operation fails after purging, and you run without `--refresh`, the entry is gone from the cache. I think that's correct behavior.

An alternative would be to try and find a way to signal that we should skip the first read, and then make cache writes everywhere safe when an entry exists already.(This latter thing is a little tricky. We could be writing a directory to the cache, which would need to replace an existing directory.)

---

_Review requested from @zanieb by @charliermarsh on 2024-01-17 15:17_

---

_Comment by @charliermarsh on 2024-01-17 15:24_

Another downside is that running with `--refresh-package flask` will only refresh the cache entries that we ultimately try to read. So you could have some mix of entries from different runs of `puffin`. That should be okay, but it's not as clear-cut as the previous behavior.

(An alternative solution to solve _that_ problem would be to structure the refresh mechanism as: the first time we ask for a package, purge _all_ entries for that package. But I think that's even more error-prone since we're then relying on `purge` including the _current_ entry, whereas this solution at least creates an explicit connection between the entry and the refresh action.)


---

_Review comment by @BurntSushi on `crates/puffin-cache/src/lib.rs`:106 on 2024-01-17 15:26_

Perhaps a dumb question, but why is the default policy to refresh everything? From your PR comment, I think my understanding is that this corresponds to the `--reinstall` flag (for `pip sync` anyway), but that flag is something the user opts into.

---

_Review comment by @BurntSushi on `crates/puffin-cache/src/lib.rs`:153 on 2024-01-17 15:35_

Thought: Can we replace _all_ extant uses of `Cache::entry` with `Cache::fresh_entry` and then rename `Cache::fresh_entry` to `Cache::entry`? (And the existing `Cache::entry` would become an internal helper routine or just deleted entirely.)

The benefit of this is that then you don't have to worry about choosing the right one. And on top of that, it seems like a minor footgun for `Cache::entry` to exist _and_ specifically ignore the configured refresh policy.

If we do need to keep `Cache::entry`, I might suggest renaming it. Maybe, `Cache::entry_no_refresh` or something similarly baroque. And then rename `fresh_entry` to `entry`, because it seems like `fresh_entry` is probably what one ought to reach for by default?

---

_@BurntSushi reviewed on 2024-01-17 15:35_

---

_@charliermarsh reviewed on 2024-01-17 15:40_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/lib.rs`:106 on 2024-01-17 15:40_

(Sorry, this should be `Refresh::None`, this was just for testing.)

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/lib.rs`:153 on 2024-01-17 15:40_

Yeah we can do something like this. There are a few cases where we can skip the refresh, but they should be done carefully.

---

_@charliermarsh reviewed on 2024-01-17 15:40_

---

_Comment by @BurntSushi on 2024-01-17 15:43_

Popping up a level, I think given the specified behavior of `--reinstall` and `--reinstall-package` (especially the latter), this largely makes sense. I do have a couple questions though:

* Do we need to support `--reinstall-package`? Do you know under what conditions is it typically used?
* As a general wonderingment, could the `Refresh::All` policy be implemented by purging the cache up-front in its entirety? If so, does that simplify anything here?

---

_Comment by @charliermarsh on 2024-01-17 15:48_

> As a general wonderingment, could the `Refresh::All` policy be implemented by purging the cache up-front in its entirety? If so, does that simplify anything here?

It could, but is that correct? It feels a bit heavy, since we'll throw away source distributions and such for other projects. We would still need this same mechanism for per-package refresh, but we could also decide that isn't important


---

_Comment by @charliermarsh on 2024-01-17 15:52_

> Do we need to support --reinstall-package? Do you know under what conditions is it typically used?

Maybe not. This was originally added to support use-cases in which users want to force a rebuild of some dependency, ignoring the version that's already installed in their environment.

For example, if you install a path dependency into your virtual environment, then by default, if you try to do it again (even if the path dependency changes in some way), we won't reinstall it. So you need to use `--reinstall` to force that package to be removed and then rebuilt.


---

_Comment by @BurntSushi on 2024-01-17 15:53_

> > As a general wonderingment, could the `Refresh::All` policy be implemented by purging the cache up-front in its entirety? If so, does that simplify anything here?
> 
> It could, but is that correct? It feels a bit heavy, since we'll throw away source distributions and such for other projects. We would still need this same mechanism for per-package refresh, but we could also decide that isn't important

Yeah I think that's kind of where I was going. That is, if we chose not to support `--reinstall-package` (or otherwise anything that purges the cache on a package-by-package basis), then that might simplify things here.

I am also asking questions to help improve my understanding, so my suggestions are not fully informed. :)

---

_Comment by @BurntSushi on 2024-01-17 15:55_

> For example, if you install a path dependency into your virtual environment, then by default, if you try to do it again (even if the path dependency changes in some way), we won't reinstall it. So you need to use `--reinstall` to force that package to be removed and then rebuilt.

Hmmm I see. That makes sense. (I was about to ask if we could just do the reinstall automatically, but you'd still need this PR to do that. And perhaps those sorts of questions are better left to the higher level commands in puffin that have yet to be written. :))

---

_Comment by @konstin on 2024-01-17 17:42_

Putting the refresh in `entry` sounds good.

Is the choice to ignore the option to make revalidation requests (for me, they are like fresh requests with a 304 Not Modified fast path) instead of fully fresh requests intentional?

---

_Comment by @charliermarsh on 2024-01-17 17:43_

@konstin - We can _maybe_ support that... I will try. It's just harder.

---

_Comment by @konstin on 2024-01-17 17:52_

My mental model is that there are four possible actions, given an existing cache entry:
* Use the cache: Fastest, possible outdated
* Refresh with default http semantics (what the cached client does): May send a fresh request, may send a revalidation request, may be offline, depending on http caching semantics
* Send a revalidation request with http semantics: We get 304 Not Modified or a fresh response, usually the 304 is fast
* Send a fresh request: Forced by purging the cache

The average cost is: fresh request >> revalidation request >> using the cache. 

---

I'm fine with any solution that doesn't depend on 5-10 min timeouts, which would give the illusion that the software is fast in warm cache cases, while in practice when i do one packaging now and another one an hour is get a much slower warm cache case.

---

_Comment by @charliermarsh on 2024-01-17 18:04_

I genuinely think that having a 10 minute HTTP cache is a totally reasonable behavior.

---

_Comment by @charliermarsh on 2024-01-17 18:09_

I view it more from the perspective of the actual user experience, and less from the perspective of the perception or illusion of the benchmarks or whatever else. It's very common to run multiple commands and operations in a short window of time. It's good to improve that experience.


---

_Closed by @charliermarsh on 2024-01-23 03:25_

---

_Comment by @charliermarsh on 2024-01-23 03:25_

This approach has some issues, refresh will come in a separate PR with a new strategy.

---
