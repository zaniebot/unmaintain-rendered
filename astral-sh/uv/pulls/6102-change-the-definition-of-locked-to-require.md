```yaml
number: 6102
title: "Change the definition of `--locked` to require satisfaction check"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/locked
created_at: 2024-08-15T01:24:39Z
updated_at: 2024-08-15T12:17:30Z
url: https://github.com/astral-sh/uv/pull/6102
synced_at: 2026-01-12T16:07:12Z
```

# Change the definition of `--locked` to require satisfaction check

---

_@charliermarsh_

## Summary

This PR changes the definition of `--locked` from:

> Produces the same `Lock`

To:

> Passes `Lock::satisfies`

This is a subtle but important difference. Previous, if `Lock::satisfies` failed, we would run a resolution, then do `existing_lock == lock`. If the two weren't equal, and `--locked` was specified, we'd throw an error.

The equality check is hard to get right. For example, it means that we can't ship #6076 without changing our marker representation, since the deserialized lockfile "loses" some of the internal marker state that gets accumulated during resolution.

The downside of this change is that there could be scenarios in which `uv lock --locked` fails even though the lockfile would actually work and the exact TOML would be unchanged. But... I think it's ok if `--locked` fails after the user modifies something?


---

_Marked ready for review by @charliermarsh on 2024-08-15 01:24_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-08-15 01:24_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-15 01:24_

---

_Label `cli` added by @charliermarsh on 2024-08-15 01:24_

---

_Label `preview` added by @charliermarsh on 2024-08-15 01:24_

---

_Comment by @charliermarsh on 2024-08-15 01:26_

I'm now trying to think of a situation in which the user could change something (like a workspace requirement), and `Lock::satisfies` fails, but then the TOML is totally unchanged. Is that even possible anymore?


---

_Comment by @charliermarsh on 2024-08-15 01:26_

Anyway, I think this is another change that would reduce a lot of complexity for us while being basically the same for users.

---

_Comment by @ibraheemdev on 2024-08-15 01:29_

> The downside of this change is that there could be scenarios in which uv lock --locked fails even though the lockfile would actually work and the exact TOML would be unchanged. 

I'm not sure I understand this, shouldn't this change allow strictly *more* lockfiles to satisfy `--locked`?

---

_Comment by @charliermarsh on 2024-08-15 01:31_

> I'm not sure I understand this, shouldn't this change allow strictly more lockfiles to satisfy --locked?

I don't think so, because before, if you failed `Lock::satisfies`, we'd still try to resolve, then compare the contents of the lockfiles. Now, if you fail `Lock::satisfies`, we'll error without trying to resolve. I think the new condition is stricter.

---

_Comment by @charliermarsh on 2024-08-15 01:36_

Does that make sense?

---

_Comment by @ibraheemdev on 2024-08-15 01:42_

I see, that makes sense. So now we're verifying against the requirements, not the new lockfile.

---

_Comment by @charliermarsh on 2024-08-15 01:45_

Yeah, that's a fair description

---

_Comment by @ibraheemdev on 2024-08-15 01:55_

I'm not sure how this compares to changing the markers to not store the range restriction state. Also trying to think of an example where the requirements change but a resolution would have resulted in the same lockfile (so `--locked` would now fail where it previously succeeded). I guess if you relaxed a version range for a package that is also depended on transitively, so the locked version does not change.. but with `requires-dist` now I guess that also changes the lockfile?

---

_Comment by @charliermarsh on 2024-08-15 01:56_

> I guess if you relaxed a version range for a package that is also depended on transitively, so the locked version does not change.. but with requires-dist now I guess that also changes the lockfile?

Yeah this is where I'm ending up too.

---

_@ibraheemdev approved on 2024-08-15 02:02_

I think the part about `requires-dist` making the lockfile change where it previously wouldn't is a little unfortunate, but if we decide to do that (separate from this PR) then this makes sense to me.

---

_Comment by @charliermarsh on 2024-08-15 03:06_

> I think the part about `requires-dist` making the lockfile change where it previously wouldn't is a little unfortunate, but if we decide to do that (separate from this PR) then this makes sense to me.

I'm wondering if this is really that big of a problem. If we don't go through `Lock::satisfies`, we still use the lockfile as preferences and the user may even have a warm cache. It's probably still super fast 🤔 


---

_Comment by @charliermarsh on 2024-08-15 03:09_

Maybe just to show my hand a bit: I think this fast path is _critical_ for `uv run`, where the vast majority of invocations won't have experienced any lockfile changes, and we want as little overhead as we can get there. Everywhere else, I view it as more of a nice-to-have...


---

_Comment by @charliermarsh on 2024-08-15 03:12_

Maybe one thing for me to think through tomorrow -- what happens when:

- User clones a repo, adds a dependency to the `pyproject.toml` and runs `uv lock`.
- User clones a repo, runs `uv sync`, then later adds a dependency to the `pyproject.toml` and runs `uv lock`.

What do we reuse? Where are we being inefficient?


---

_Comment by @ibraheemdev on 2024-08-15 03:27_

The other thing we lost is user clones a repo and runs `uv lock` with a cold cache (again somewhat unrelated to this change).

---

_Comment by @charliermarsh on 2024-08-15 03:32_

I think that’s unchanged, no? What’s different there?

---

_Comment by @ibraheemdev on 2024-08-15 03:42_

Sorry, you're right, that hasn't changed. Adding a dependency is where it changed.

---

_Comment by @charliermarsh on 2024-08-15 03:45_

Yeah, adding a dependency (that was already in the lockfile, I think? But not explicit) changed.

---

_Comment by @ibraheemdev on 2024-08-15 04:10_

Any resolution that didn't require fetching versions of existing packages that were not in the lockfile changed. For example, adding a new dependency that did not affect the locked versions of any existing dependencies could use the prefilled index from the lockfile and only fetch the metadata for the new package.

Not sure how big of an impact that optimization had though, and I think we can still make it in the future.

---

_@BurntSushi approved on 2024-08-15 12:13_

I think we should do this because it moves `uv` to a more conservative posture. But I do think we should improve on this in the future. Some specific thoughts:

> The equality check is hard to get right. For example, it means that we can't ship https://github.com/astral-sh/uv/pull/6076 without changing our marker representation, since the deserialized lockfile "loses" some of the internal marker state that gets accumulated during resolution.

Is this something that could also be fixed by a custom equality comparison?

> The downside of this change is that there could be scenarios in which uv lock --locked fails even though the lockfile would actually work and the exact TOML would be unchanged. But... I think it's ok if --locked fails after the user modifies something?

I agree. Or at least, I think this is the right decision to make right now.

> I'm now trying to think of a situation in which the user could change something (like a workspace requirement), and Lock::satisfies fails, but then the TOML is totally unchanged. Is that even possible anymore?

One example is:

* In `pyproject.toml`, there is a dependency `foo>=1.0`.
* In `uv.lock`, `foo` is locked to `1.2`.
* I make a change to `pyproject.toml` by updating the dependency on `foo` to `foo>=1.1`.

This is a somewhat strained example, but it's definitely something I've done a few times in the Rust world where I realized my minimum version constraint was wrong (because I was depending on features in `foo` released in `1.1`). So it's not like it never happens. But I think this is a case where `Lock::satisfies` will fail, but a re-resolve (assuming nothing in the world changed) should produce the same _semantic_ `uv.lock`. (The actual TOML would be different because we write out the full requirements, and that changed, but the actual locked version that we install wouldn't change.)

But like, I don't necessarily think that is a major issue. Or at least, not a release blocking issue. Unless there are other more important use cases that I'm missing. That is, I think the worst thing that happens here is that folks might need to re-run `uv lock` in some cases where they might not otherwise have to.

---

_Comment by @charliermarsh on 2024-08-15 12:17_

👍 I agree with the above. (And yeah, the TOML _would_ change in that format, but the locked dependencies would not.)

---

_Merged by @charliermarsh on 2024-08-15 12:17_

---

_Closed by @charliermarsh on 2024-08-15 12:17_

---

_Branch deleted on 2024-08-15 12:17_

---
