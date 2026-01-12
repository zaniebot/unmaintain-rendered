```yaml
number: 10335
title: "Add Git LFS support to `uv-git` crate"
type: pull_request
state: merged
author: sydduckworth
labels: []
assignees: []
merged: true
base: main
head: git-lfs-support
created_at: 2025-01-06T21:25:18Z
updated_at: 2025-01-13T21:48:07Z
url: https://github.com/astral-sh/uv/pull/10335
synced_at: 2026-01-12T16:09:14Z
```

# Add Git LFS support to `uv-git` crate

---

_@sydduckworth_

## Summary

Closes #3312.

This PR adds Git LFS support to the `uv-git` crate by using the `git-lfs` CLI to fetch required LFS objects for a revision following the call to `git fetch`.

The LFS fetch step is disabled by default and only enabled if the environment variable `UV_GIT_LFS` is set.

When enabled, the LFS fetch step is run for all repositories regardless of whether they have associated LFS objects. The step is skipped if the `git-lfs` CLI tool isn't installed.

## Test Plan

I verified that the minimal example in the linked issue passes, i.e. this command now succeeds:

```sh
UV_GIT_LFS=1 uv pip install git+https://github.com/grebnetiew/lfs-py.git
```

I also verified that non-LFS repositories still work, with or without `git-lfs` installed.

### To Replicate
Attempt to use uv to install a Git dependency that contains LFS objects (e.g. `uv pip install git+https://github.com/grebnetiew/lfs-py.git`). This should fail with a smudge filter error.

Re-run the same command with the added environment variable `UV_GIT_LFS=1`. The install should now succeed.

## Potential Changes / Improvements

~With this change LFS objects in a given revision will always be downloaded if the user has Git LFS installed, which may not always be desired behavior. It might be helpful to add a field to the `uv` settings and/or an environment variable so that the LFS step can be disabled if needed.~

Enabling/disabled via environment variable has now been implemented.

---

_@samypr100 reviewed on 2025-01-07 19:39_

---

_Review comment by @samypr100 on `crates/uv-git/src/git.rs`:45 on 2025-01-07 19:39_

I'm not certain this detection method is portable to all end user systems. For example, I can have git lfs installed and working but I not have git-lfs binary on the path. I think probing `git lfs` directly is likely the most portable approach.

---

_@sydduckworth reviewed on 2025-01-07 20:27_

---

_Review comment by @sydduckworth on `crates/uv-git/src/git.rs`:45 on 2025-01-07 20:27_

Okay, I've updated the logic so now instead of searching for `git-lfs` on the path, it just tries to run `git lfs version` the first time LFS is invoked and uses the success of that command as an indicator of LFS availability.

I'd definitely be interested in if anyone has a cleaner way of determining whether LFS is available. I was initially going to just check if the LFS fetch command returned `127` (command not found), but that return code isn't available in Powershell, so that approach isn't portable across shells.

---

_Review requested from @samypr100 by @sydduckworth on 2025-01-09 15:43_

---

_Comment by @samypr100 on 2025-01-12 16:27_

> With this change LFS objects in a given revision will always be downloaded if the user has Git LFS installed, which may not always be desired behavior. It might be helpful to add a field to the uv settings and/or an environment variable so that the LFS step can be disabled if needed.

Agree, since this is technically a possible breaking change a way to `opt-in` would be great. I think an env var can be used here to trigger the behavior change as its relatively simple. I'd rather have this be opt-in than opt-out as fetching lfs objects may not always be desired, particularly on large downloads.

Thoughts @zanieb?

---

_Comment by @sydduckworth on 2025-01-12 17:24_

@samypr100 I've updated the PR so that the LFS fetch step is off by default, and is enabled by setting the environment variable `UV_GIT_LFS`.

---

_Comment by @zanieb on 2025-01-12 18:09_

Opt-in is a great idea — it makes us much more likely to accept the feature.

---

_Review requested from @zanieb by @zanieb on 2025-01-12 18:09_

---

_Assigned to @zanieb by @zanieb on 2025-01-12 18:09_

---

_Comment by @zanieb on 2025-01-12 18:10_

Did you do any benchmarking of this?

---

_Comment by @sydduckworth on 2025-01-12 18:59_

Testing on my local system with a repository with a small number of tracked files (`git+https://github.com/astral-test/uv-public-pypackage`), I couldn't produce a statistically significant difference in execution time for `uv pip install` with or without LFS support enabled.

That said, the added overhead *should* be equal to the time required to run `git lfs version` once (4.6 ms on my system) plus the time required to run `git lfs fetch` for each repository (15 ms for the test repo on my system). Execution time of `git lfs fetch` scales with number of tracked files but even testing with larger repositories I got about the same result.  
So the predicted overhead (on my machine) with LFS enabled (assuming the command interacts with at least one Git repository) would be about 5 ms + about 15 ms per Git repository.

---

_Comment by @samypr100 on 2025-01-12 19:18_

~~You might be able to eliminate the 5ms overhead by moving the env var check before the lfs check~~ nevermind, I hadn't seen the latest code.

---

_Merged by @zanieb on 2025-01-13 21:48_

---

_Closed by @zanieb on 2025-01-13 21:48_

---
