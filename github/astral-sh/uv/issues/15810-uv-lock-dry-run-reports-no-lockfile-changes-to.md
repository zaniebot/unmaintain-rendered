---
number: 15810
title: "`uv lock --dry-run` reports \"No lockfile changes\" to VCS-sourced packages"
type: issue
state: closed
author: tehunter
labels:
  - bug
assignees: []
created_at: 2025-09-12T13:32:11Z
updated_at: 2025-09-12T17:57:59Z
url: https://github.com/astral-sh/uv/issues/15810
synced_at: 2026-01-07T13:12:19-06:00
---

# `uv lock --dry-run` reports "No lockfile changes" to VCS-sourced packages

---

_Issue opened by @tehunter on 2025-09-12 13:32_

### Summary

## Summary

When using VCS-based dependency specifications, `uv lock --dry-run -P [package]` reports a misleading "No lockfile changes detected" while `uv lock -P [package]` does change the lockfile to reference the latest commit hash.

## Details

I have a project dependency which is a git-based package, e.g.:
```
[project]
dependencies = [
    "mypackage @ git+https://github.com/username/mypackage"
]
```

The lock file is currently locked to a old commit, e.g.:
```
[[package]]
name = "mypackage"
version = "0.0.1"
source = { git = "https://github.com/username/mypackage#123456" }
```

I want to run `uv lock --dry-run -P mypackage` to verify that it will pull in the latest commit. When I do, I receive the following output:

```
uv lock --dry-run -P mypackage
    Updated https://github.com/username/mypackage (abcdef...)
Resolved X packages in Ys
No lockfile changes detected
```

However, when I run without `--dry-run`, the lock file does in fact update and reference the new commit.

It should be noted that I haven't done anything to change the version of "mypackage" between these two commits. However, I would expect that VCS-based dependencies are locked based on a commit hash rather than version number.

The message that "No lockfile changes detected" is misleading here. I would interpret that to mean the lock file would not change at all with a regular run.

## Expected Output

```
$ uv lock --dry-run -P mypackage
    Updated https://github.com/username/mypackage (abcdef...)
Resolved X packages in Ys
Update mypackage v0.0.1 (123456) -> v0.0.1 (abcdef)
```

### Platform

Windows 11 x64

### Version

uv 0.8.17 (10960bc13 2025-09-10)

### Python version

Python 3.12.7

---

_Label `bug` added by @tehunter on 2025-09-12 13:32_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-09-12 15:05_

---

_Comment by @charliermarsh on 2025-09-12 16:20_

I think this is because the `--dry-run` changelog only logs based on package version (and the actual package version didn't change). We can add a commit SHA to it, I think.

---

_Referenced in [astral-sh/uv#15817](../../astral-sh/uv/pulls/15817.md) on 2025-09-12 16:57_

---

_Closed by @charliermarsh on 2025-09-12 17:57_

---
