```yaml
number: 12850
title: Add missing git feature dep to preserve_executable_bit test
type: pull_request
state: merged
author: mgorny
labels:
  - bug
  - internal
assignees: []
merged: true
base: main
head: git-feat
created_at: 2025-04-12T11:06:47Z
updated_at: 2025-05-18T21:31:02Z
url: https://github.com/astral-sh/uv/pull/12850
synced_at: 2026-01-12T16:10:24Z
```

# Add missing git feature dep to preserve_executable_bit test

---

_@mgorny_

## Summary

Without the `git` feature, it fails with:

```
error: Failed to initialize Git repository at `/home/mgorny/.local/share/uv/tests/.tmp01wGGK/temp/preserve_executable_bit`
stdout:
stderr: error: `git` operations are not allowed — are you missing a cfg for the `git` feature?
```

## Test Plan
    cargo test --features python --profile=fast-build --no-default-features


---

_Comment by @charliermarsh on 2025-04-14 12:28_

@konstin -- Do you know why this test uses Git? I think `uv init` is meant to be robust to a missing `git`.

---

_Label `bug` added by @charliermarsh on 2025-04-14 12:29_

---

_Comment by @konstin on 2025-04-14 13:07_

We first check if we are in a git repository in `uv init`. If that fails, we fall back to assuming VCS defaults (init a git repo). We don't check the error condition, so both `fatal: not a git repository (or any of the parent directories): .git` and our git blocking shim cause this path. We then try to init a git repo, but in that case we only ignore a missing git installation, but fail at the git blocking shim.

Can we rely on errors such as `not a git repository` being stable? If so, we could differentiate between the two cases.

---

_Label `internal` added by @konstin on 2025-04-14 13:07_

---

_Comment by @charliermarsh on 2025-04-14 13:11_

But doesn't "init a Git repo" return `VersionControlError::GitNotInstalled`, which we explicitly handle?

---

_Comment by @konstin on 2025-04-14 14:27_

Our shims put a `git` in `PATH` so we're falling in a gap where git is installed (`which git` find the git-not-allowed shim), but invoking git errors with the error seen above:

First we take the `None`-branch, since detecting an existing git repo errors (through the shim):

https://github.com/astral-sh/uv/blob/0c801f8f4bd187a1555b029e6e1e9b7332b0971c/crates/uv-configuration/src/vcs.rs#L97

But that was a check for an existing git repo, so we ignore it:

https://github.com/astral-sh/uv/blob/42dcea0ee2a38d949e54e05a2ef1608f9f5fff73/crates/uv/src/commands/project/init.rs#L1203

When we finally initialize the repo, we can run the git shim, but it errors on execution:

https://github.com/astral-sh/uv/blob/0c801f8f4bd187a1555b029e6e1e9b7332b0971c/crates/uv-configuration/src/vcs.rs#L52

If we want to handle this better, we should find a way to better tell apart "there is no git repo" (this is fine) from "git errored" (don't use git at all) in the first check.

---

_Comment by @konstin on 2025-04-15 10:52_

I tried to make this more resilient: #12895

---

_Merged by @charliermarsh on 2025-05-18 21:31_

---

_Closed by @charliermarsh on 2025-05-18 21:31_

---
