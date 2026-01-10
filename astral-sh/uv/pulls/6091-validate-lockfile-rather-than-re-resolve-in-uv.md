```yaml
number: 6091
title: "Validate lockfile (rather than re-resolve) in `uv lock`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - lock
assignees: []
merged: true
base: main
head: charlie/val
created_at: 2024-08-14T18:21:30Z
updated_at: 2024-08-15T00:00:17Z
url: https://github.com/astral-sh/uv/pull/6091
synced_at: 2026-01-10T13:09:50Z
```

# Validate lockfile (rather than re-resolve) in `uv lock`

---

_Pull request opened by @charliermarsh on 2024-08-14 18:21_

## Summary

Historically, in order to "resolve from a lockfile", we've taken the lockfile, used it to pre-populate the in-memory metadata index, then run a resolution. If the resolution didn't match our existing resolution, we re-resolved from scratch.

This was an appealing approach because (in theory) it didn't require any dedicated logic beyond pre-populating the index. However, it's proven to be _really_ hard to get right, because it's a stricter requirement than we need. We just need the current lockfile to _satisfy_ the requirements provided by the user. We don't actually need a second resolution to produce the exact same result. And it's not uncommon that this second resolution differs, because we seed it with preferences, which fundamentally changes its course. We've worked hard to minimize those "instabilities", but they're still present.

The approach here is intended to be much simpler. Instead of resolving from the lockfile, we just check if the current resolution satisfies the state of the workspace. Specifically, we check if the lockfile (1) contains all the relevant members, and (2) matches the metadata for all dependencies, recursively. (We skip registry dependencies, assuming that they're immutable.)

This may actually be too conservative, since we can have resolutions that satisfy the requirements, even if the requirements have changed slightly. But we want to bias towards correctness for now.

My hope is that this scheme will be more performant, simpler, and more robust.

Closes https://github.com/astral-sh/uv/issues/6063.


---

_Review comment by @BurntSushi on `crates/uv/tests/common/mod.rs`:52 on 2024-08-14 21:13_

Should this be a STOPSHIP?

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:738 on 2024-08-14 21:19_

I think these are all fine, but I think will result in reporting "not satisfied" in cases where the lock file does actually satisfy the requirements. But I think this is something we can improve in follow-up PRs. Specifically, even if the dependency specification in the `pyproject.toml` changes, the version that's in the lock file can still satisfy it such that no actual updates are needed. But I think this implementation will say an update is needed if _any_ change occurs.

---

_@BurntSushi reviewed on 2024-08-14 21:19_

I feel like we should move forward in this direction. I still think there's probably an opportunity to limit what we need to write to the lock file, but I'd rather be in a position where our lock file is more bloated than it needs to be but we're more confident in its stability.

---

_@BurntSushi reviewed on 2024-08-14 21:19_

---

_Review comment by @BurntSushi on `crates/uv/tests/common/mod.rs`:52 on 2024-08-14 21:19_

I mean, going back to doing deterministic checking.

---

_@charliermarsh reviewed on 2024-08-14 21:24_

---

_Review comment by @charliermarsh on `crates/uv/tests/common/mod.rs`:52 on 2024-08-14 21:24_

I replaced all the usages of this in `lock` with a second call to `.lock` with `--locked`. The `deterministic!` macro just makes it really hard to update the snapshots since you have to manually edit the tests, and `--locked` will fail more aggressively since it doesn't _just_ require that the string contents changed.

(I've uncommented this though, and left it in the ecosystem checks.)

---

_@charliermarsh reviewed on 2024-08-14 21:25_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:738 on 2024-08-14 21:25_

Yeah this may actually be too conservative... I mostly care about this capability to ensure that `uv run` is really fast / cheap, so I'm ok with having to do a resolution if the user edits the dependencies.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-08-14 21:29_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-14 21:29_

---

_Label `internal` added by @charliermarsh on 2024-08-14 21:29_

---

_Label `lock` added by @charliermarsh on 2024-08-14 21:29_

---

_Marked ready for review by @charliermarsh on 2024-08-14 21:30_

---

_Comment by @charliermarsh on 2024-08-14 21:30_

I think the code changes here might be roughly a wash? But I added a lot of tests.

---

_Comment by @charliermarsh on 2024-08-14 21:31_

https://github.com/astral-sh/uv/issues/6063 no longer occurs.

---

_@ibraheemdev reviewed on 2024-08-14 21:34_

---

_Review comment by @ibraheemdev on `crates/uv/tests/common/mod.rs`:52 on 2024-08-14 21:34_

Does removing the `with_duplicates` fix the snapshot updating issue?

---

_@ibraheemdev approved on 2024-08-14 21:42_

It's a little unfortunate that we can't rely on the lockfile for metadata since that had a lot of potential in terms of performant incremental resolves (though I'm not sure this PR necessarily removes that as a future option). However, I agree that this approach is more obviously correct and probably our best option.

---

_Comment by @charliermarsh on 2024-08-14 21:56_

It's possible that we could still support that... But the framing would have to be a little different. Like, our goal would be to use metadata from the lockfile in the _normal_ resolve, not the noop resolve.

---

_@BurntSushi approved on 2024-08-14 22:11_

Unfortunate, but I think this is the right way to go.

Also, you did this _crazy_ fast.

---

_@charliermarsh reviewed on 2024-08-14 22:29_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/requirement.rs`:728 on 2024-08-14 22:29_

Subtly, this is the worst part of the PR.

---

_Merged by @charliermarsh on 2024-08-15 00:00_

---

_Closed by @charliermarsh on 2024-08-15 00:00_

---

_Branch deleted on 2024-08-15 00:00_

---
