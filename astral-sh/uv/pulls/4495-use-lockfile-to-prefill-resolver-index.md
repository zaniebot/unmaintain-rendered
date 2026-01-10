```yaml
number: 4495
title: Use lockfile to prefill resolver index
type: pull_request
state: merged
author: ibraheemdev
labels:
  - performance
  - preview
assignees: []
merged: true
base: main
head: ibraheem/lock-resolver
created_at: 2024-06-24T22:50:17Z
updated_at: 2024-07-12T22:49:30Z
url: https://github.com/astral-sh/uv/pull/4495
synced_at: 2026-01-10T13:42:52Z
```

# Use lockfile to prefill resolver index

---

_Pull request opened by @ibraheemdev on 2024-06-24 22:50_

## Summary

Use the lockfile to prefill the `InMemoryIndex` used by the resolver. This enables us to resolve completely from the lockfile without making any network requests/builds if the requirements are unchanged. It also means that if new requirements are added we can still avoid most I/O during resolution, partially addressing https://github.com/astral-sh/uv/issues/3925.

The main limitation of this PR is that resolution from the lockfile can fail if new versions are requested that are not present in the lockfile, in which case we have to perform a fresh resolution. Fixing this would likely require lazy version/metadata requests by `VersionMap` (this is different from the lazy parsing we do, the list of versions in a `VersionMap` is currently immutable).

Resolves https://github.com/astral-sh/uv/issues/3892.

## Test Plan

Added a `deterministic!` macro that ensures that a resolve from the lockfile and a clean resolve result in the same lockfile output for all our current tests.

---

_Comment by @ibraheemdev on 2024-06-24 22:53_

One thing I'm wondering if any of the metadata stored in the lockfile is potentially mutable, meaning resolution would fail in the presence of a lockfile. e.g. we probably can't rely on the locked metadata of path dependencies, and maybe also direct URLs?

---

_@ibraheemdev reviewed on 2024-06-25 17:50_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/lock.rs`:883 on 2024-06-25 17:50_

A little concerned about reconstructing the `MarkerTree` like this without knowing the original order, but because we remove it when reconstructing the lockfile I think it's fine.

---

_@ibraheemdev reviewed on 2024-06-25 18:01_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/project/lock.rs`:280 on 2024-06-25 18:01_

Should stderr be suppressed here? Thinking there might be duplicate warnings that might be printed if resolution from a lockfile fails.

---

_@ibraheemdev reviewed on 2024-06-25 18:01_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/project/lock.rs`:286 on 2024-06-25 18:01_

Right now this assumes that `Lock::from_resolution_graph` won't fail on the resolution from a lockfile where it would otherwise succeed.. I think that makes sense but not 100% sure.

---

_Marked ready for review by @ibraheemdev on 2024-06-25 18:02_

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-06-25 18:02_

---

_Review requested from @BurntSushi by @ibraheemdev on 2024-06-25 18:02_

---

_Comment by @charliermarsh on 2024-06-25 22:45_

> One thing I'm wondering if any of the metadata stored in the lockfile is potentially mutable, meaning resolution would fail in the presence of a lockfile. e.g. we probably can't rely on the locked metadata of path dependencies, and maybe also direct URLs?

Yeah, this is the right instinct. We need to check if they're up-to-date... Take a look at `RequirementSatisfaction` which performs these checks when (e.g.) determining whether we can use a value from the cache.

In general, we assume that HTTP direct URL requirements are immutable and thus require `--upgrade` or `--refresh` or similar. For file-based direct URL requirements, the rules are a little more nuanced.


---

_@charliermarsh reviewed on 2024-06-25 23:43_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:280 on 2024-06-25 23:43_

Yeah, we probably want to suppress everything here so that if resolution succeeds, we don't log anything about it.

---

_@charliermarsh reviewed on 2024-06-26 00:20_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/upgrade.rs`:70 on 2024-06-26 00:20_

Maybe we split this into two functions, one that reads the lockfile, and one that takes `Lock` and returns `LockedRequirements`? Seems more flexible to compose that way.

---

_@charliermarsh reviewed on 2024-06-26 00:21_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:883 on 2024-06-26 00:21_

Yeah I think the metadata we're constructing here is potentially incorrect... But perhaps not in a way that matters? It's hard to reason about. Like, we won't list all the extras here so `Provides-Extras` will be wrong -- we'll only include the extras that were activated when resolving...

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:288 on 2024-06-26 13:10_

For the same package name, yes, I think so?

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:883 on 2024-06-26 13:24_

Yeah it makes some sense to me that we're doing this. I think it's adding back what we do here?

https://github.com/astral-sh/uv/blob/b677a06abad58868e226e19e722842f9bd8ec816/crates/pypi-types/src/metadata.rs#L254

Which reminds me: is it possible to simplify this code a bit via `MarkerTree::with_extra_marker`?

https://github.com/astral-sh/uv/blob/b677a06abad58868e226e19e722842f9bd8ec816/crates/pep508-rs/src/lib.rs#L412-L435

---

_Review comment by @BurntSushi on `crates/uv/src/commands/project/lock.rs`:286 on 2024-06-26 13:32_

That's plausible I think, but definitely feels pretty difficult to guarantee. There are a lot of error cases.

What happens if the assumption is wrong?

---

_Review comment by @BurntSushi on `crates/uv/src/commands/project/lock.rs`:247 on 2024-06-26 13:33_

Nit: I like to have shorter cases like this toward the top. It helps with a linear reading of the code. Otherwise, if you're reading this, I find I have to scroll back up to see where that `None` is coming from.

---

_Review comment by @BurntSushi on `crates/uv/tests/lock.rs`:30 on 2024-06-26 13:34_

Cute.

---

_Review comment by @BurntSushi on `crates/uv/tests/lock.rs`:30 on 2024-06-26 13:36_

Looking at this commit, is the only change that `deterministic! { ... }` is added to each of the tests? It's not easy to tell if that's the case from the diff.

---

_@BurntSushi reviewed on 2024-06-26 13:37_

I think this LGTM with some tests!

---

_@ibraheemdev reviewed on 2024-06-26 16:01_

---

_Review comment by @ibraheemdev on `crates/uv/tests/lock.rs`:30 on 2024-06-26 16:01_

Yeah, the git diff is pretty confusing but that's the only change. https://app.semanticdiff.com/gh/astral-sh/uv/pull/4495/changes#crates/uv/tests/lock.rs.

---

_@ibraheemdev reviewed on 2024-06-26 16:04_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/project/lock.rs`:286 on 2024-06-26 16:04_

If it's wrong we run into error cases because the metadata we construct from the lock file is invalid, and don't fallback to a clean resolve. However, silently falling back would mean potentially hiding bugs in `lock.to_index`, so I think this is better unless we run into concrete issues down the line?

---

_@ibraheemdev reviewed on 2024-06-26 16:05_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/project/lock.rs`:280 on 2024-06-26 16:05_

I'm not sure we should supress everything because this resolve might still involve downloading/building new distributions?

---

_@ibraheemdev reviewed on 2024-06-26 16:06_

---

_Review comment by @ibraheemdev on `crates/uv/tests/lock.rs`:30 on 2024-06-26 16:06_

Do you think it needs more tests than this change?

---

_@ibraheemdev reviewed on 2024-06-26 16:27_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/lock.rs`:883 on 2024-06-26 16:27_

Looks like that's `pep508_rs::Requirement::with_extra_marker` and we need `pypi_types::Requirement::with_extra_marker`. Ideally this could be on `MarkerTree` but the `Option` gets in the way..

---

_Comment by @ibraheemdev on 2024-06-26 17:54_

On the airflow benchmark, which is a very large lockfile, this speeds up `uv lock`  by 2x:

```
$ hyperfine "../uv/target/profiling/baseline lock" "../uv/target/profiling/uv lock"
Benchmark 1: ../uv/target/profiling/baseline lock
  Time (mean ± σ):     183.6 ms ±   4.6 ms    [User: 199.8 ms, System: 124.6 ms]
  Range (min … max):   176.1 ms … 190.4 ms    15 runs
 
Benchmark 2: ../uv/target/profiling/uv lock
  Time (mean ± σ):      93.6 ms ±   2.0 ms    [User: 78.9 ms, System: 16.2 ms]
  Range (min … max):    90.5 ms …  99.1 ms    31 runs
 
Summary
  ../uv/target/profiling/uv lock ran
    1.96 ± 0.06 times faster than ../uv/target/profiling/baseline lock
```

From a user perspective, 100ms seems relatively instant while I can feel some lag at 200ms, so this is a noticeable improvement. There is probably still room for some improvement here.

The big win here is that with a cold cache, `uv lock` can still run in 100ms due to the presence of a lockfile, where it would previously take over 30 seconds.

---

_@ibraheemdev reviewed on 2024-06-26 17:58_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/lock.rs`:288 on 2024-06-26 17:58_

I think this is correct. The `Vec<VersionMap>` seems to be used when fetching from multiple indexes, but they are all merged eventually anyways. I think that distinction could be removed.

---

_@charliermarsh reviewed on 2024-06-26 18:02_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:280 on 2024-06-26 18:02_

When would we need to download / build a new distribution? The only case in which we build a distribution during resolve is to create a wheel from a source dist, but don't we already have the metadata?

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-06-26 18:03_

---

_@ibraheemdev reviewed on 2024-06-26 19:28_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/project/lock.rs`:280 on 2024-06-26 19:28_

We're still doing a regular resolve with the default provider, only existing immutable metadata is prefilled. New dependencies can still be fetched and built.

---

_@BurntSushi reviewed on 2024-06-27 12:14_

---

_Review comment by @BurntSushi on `crates/uv/src/commands/project/lock.rs`:286 on 2024-06-27 12:14_

I'm inclined to agree.

---

_@BurntSushi reviewed on 2024-06-27 12:17_

---

_Review comment by @BurntSushi on `crates/uv/tests/lock.rs`:30 on 2024-06-27 12:17_

Ah okay, I see. I think I just missed how this macro was testing this change. Perhaps a question here though is, do any of the tests fail without `deterministic!` wrapped around them?

I suppose it is perhaps difficult to construct such a test, by design. I guess I'm just wondering if there's a specific way to test this change such that the result of the test would be different if you hadn't done this prefilling. But given that it's meant to be an optimization and not a change in semantics, I'd guess probably not.

---

_@BurntSushi approved on 2024-06-27 12:17_

---

_@charliermarsh reviewed on 2024-06-28 01:16_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:280 on 2024-06-28 01:16_

Yeah not sure how to solve this. I think I'd need to see the output in various cases to develop an opinion on it...

---

_@charliermarsh reviewed on 2024-06-28 01:17_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:270 on 2024-06-28 01:17_

We probably need to do some rigorous thinking around how `reinstall` and `upgrade` work here, and whether they will be respected correctly, both for the registry distributions that are already locked and for the mutable distributions that we're omitting (like path dependencies).

---

_@charliermarsh reviewed on 2024-06-28 01:18_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:883 on 2024-06-28 01:18_

I mentioned this on Discord, but there's at least one problem with the partial metadata here. If we have some package in the lockfile (like `flask`), but it wasn't requested with an extra initially, we won't write that non-requested extra to the lockfile. If someone then comes in and asks for `flask[dotenv]`, we won't have the list of dependencies that come with `flask[dotenv]` -- we just don't store that in the lockfile at all.

So we need some solution for this. Note that non-existent extras are _not_ an error in the resolver; we just warn, mostly because extras can vary over time.

---

_@charliermarsh reviewed on 2024-06-28 01:20_

I think this is an interesting idea and we should keep pushing it.

---

_@charliermarsh reviewed on 2024-06-28 01:20_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:270 on 2024-06-28 01:20_

I think this is worth some dedicated tests or at least some exploration on the command-line.

---

_Comment by @charliermarsh on 2024-06-28 01:20_

The fact that you can resolve so quickly with a cold cache is crucial.

---

_@charliermarsh reviewed on 2024-06-28 01:23_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:280 on 2024-06-28 01:23_

I guess the only output is like... anything related to building (which we want to keep, right?)... then the summary message (which means we succeeded, so that's fine)... then warnings? But we only print warnings (1) if the resolution fails, as hints to the user (so those wouldn't be shown), and then (2) via "diagnostics" on the resolution (look for `operations::diagnose_resolution` calls). So actually this is probably totally fine as-is?

---

_Comment by @charliermarsh on 2024-07-07 19:03_

What if we intentionally run the resolver in `Offline` mode for this?

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:834 on 2024-07-08 14:00_

Is that true even when requires-python changed?

---

_@konstin reviewed on 2024-07-08 14:05_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/lock.rs`:834 on 2024-07-11 18:55_

From what I understand compatibility has to do with whether hashes matched and is used for wheel prioritization.

---

_@ibraheemdev reviewed on 2024-07-11 18:55_

---

_Comment by @ibraheemdev on 2024-07-11 20:13_

Okay, I think this is ready now.

- The upgrade strategy is respected. Any packages included in the upgrade strategy (all of them if `--upgrade`) are excluded from the prefilled index. See the passing `lock_preference` test.
- Missing extras/development groups are detected and require a fresh resolution. This unfortunately means that projects with extras that don't have optional dependencies, such as `requests[security]`, always require a fresh resolve. The solution to this is either to add extra information to the lockfile or eventually add per-dependency fallbacks in the resolver to fetch up-to-date metadata. See the passing `lock_new_extras` test.
- The duplicated `resolved in Xms` is now avoided. We still may print checkout/build progress in the initial resolve from the lockfile even if it fails, but the progress is seamless as the new progress bar overwrites the previous one in-place. You can see how this looks locally by adding a cold git dependency and running `uv lock` with a new extra added to an existing dependency.

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-07-11 20:14_

---

_Review requested from @BurntSushi by @ibraheemdev on 2024-07-11 20:14_

---

_Comment by @ibraheemdev on 2024-07-11 20:19_

One remaining issue is that we don't check for yanked releases (see https://github.com/astral-sh/uv/issues/3892#issuecomment-2212528949). I'm not sure how we should tackle this, does PyPI have a fast-path to check if a specific version was yanked? It will likely still result in significant overhead.

---

_Label `performance` added by @ibraheemdev on 2024-07-11 20:24_

---

_Label `preview` added by @ibraheemdev on 2024-07-11 20:24_

---

_Comment by @charliermarsh on 2024-07-12 01:23_

> Missing extras/development groups are detected and require a fresh resolution. This unfortunately means that projects with extras that don't have optional dependencies, such as requests[security], always require a fresh resolve. The solution to this is either to add extra information to the lockfile or eventually add per-dependency fallbacks in the resolver to fetch up-to-date metadata. See the passing lock_new_extras test.

Can you explain this piece in a little more detail? What are "projects with extras that don't have optional dependencies"? Does `security` not exist as a valid extra for `requests`?


---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:498 on 2024-07-12 01:24_

Nit: `matches!`?

---

_@charliermarsh reviewed on 2024-07-12 01:24_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/operations.rs`:101 on 2024-07-12 01:25_

Could we just pass `Printer::Quiet`?

---

_@charliermarsh reviewed on 2024-07-12 01:25_

---

_@charliermarsh reviewed on 2024-07-12 01:26_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:308 on 2024-07-12 01:26_

Can we add some debug logging for the various paths here?

---

_Comment by @charliermarsh on 2024-07-12 01:26_

This is looking good.

---

_@ibraheemdev reviewed on 2024-07-12 01:29_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/operations.rs`:101 on 2024-07-12 01:29_

We still want to output the build/download progress to stderr, we just don't want to print the final "Resolved in 20ms" in case we end up needing to resolve again because of warnings.

---

_@ibraheemdev reviewed on 2024-07-12 01:29_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/lock.rs`:498 on 2024-07-12 01:29_

I slightly prefer the exhaustive `match` here because it's clear about what we're ignoring and what we're prefilling.

---

_Comment by @ibraheemdev on 2024-07-12 01:33_

> Can you explain this piece in a little more detail? What are "projects with extras that don't have optional dependencies"? Does security not exist as a valid extra for requests?

Ah I think I was confused about this. `security` is an extra provided by `requests`, but [it doesn't enable any optional dependencies](https://github.com/psf/requests/blob/main/setup.py#L124) so it doesn't show up in the lockfile. Turns out it is deprecated so I don't think this is a problem. I was thinking that some packages might provide extras that enable features dynamically without introducing optional dependencies (like cargo features) but I don't think that's expected (or even possible)?

---

_Comment by @charliermarsh on 2024-07-12 01:41_

Ohhh ok, I see. Yes, this makes sense.

---

_@charliermarsh reviewed on 2024-07-12 01:42_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:1 on 2024-07-12 01:42_

What's more annoying, Clippy or me as reviewer?

---

_@konstin reviewed on 2024-07-12 07:29_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:834 on 2024-07-12 07:29_

Say we locked for python `==3.11.*`, and now we're re-resolving for `==3.12.*`. The lockfile will only contains `cp311-cp311` wheels. Here we're declaring these compatible in prioritized dist, even though we don't know if for python 3.12, there exist compatible wheels.

CC @charliermarsh can you check my `PrioritizedDist` claims, i sometimes get these mixed up

---

_Comment by @BurntSushi on 2024-07-12 12:15_

> I was thinking that some packages might provide extras that enable features dynamically without introducing optional dependencies (like cargo features) but I don't think that's expected (or even possible)?

Yeah I believe this is not possible. AIUI, extras in Python disappear after dependencies are resolved. They aren't themselves directly visible from the package that defines them, unlike Cargo features, which can be used in arbitrary ways.

---

_@charliermarsh reviewed on 2024-07-12 12:41_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:834 on 2024-07-12 12:41_

Can we just abort the fast-path check if `requires-python` changes?

---

_@BurntSushi approved on 2024-07-12 14:01_

Amazing!

---

_Merged by @charliermarsh on 2024-07-12 22:49_

---

_Closed by @charliermarsh on 2024-07-12 22:49_

---

_Branch deleted on 2024-07-12 22:49_

---
