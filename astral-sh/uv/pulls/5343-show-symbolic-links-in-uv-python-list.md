```yaml
number: 5343
title: "Show symbolic links in `uv python list`"
type: pull_request
state: merged
author: eth3lbert
labels: []
assignees: []
merged: true
base: main
head: python-list-symlink
created_at: 2024-07-23T16:13:15Z
updated_at: 2024-07-23T16:51:43Z
url: https://github.com/astral-sh/uv/pull/5343
synced_at: 2026-01-10T13:37:23Z
```

# Show symbolic links in `uv python list`

---

_Pull request opened by @eth3lbert on 2024-07-23 16:13_

## Summary

This PR displays symbolic links in `uv python list` and produces output similar to the following:

```
:) uv python list --preview
cpython-3.12.1-macos-aarch64-none     /Users/eth/workspace/astral-sh/uv/bin/python3.12 -> versions/cpython@3.12.1/install/bin/python3
cpython-3.12.1-macos-aarch64-none     /Users/eth/workspace/astral-sh/uv/bin/python3 -> versions/cpython@3.12.1/install/bin/python3
cpython-3.12.1-macos-aarch64-none     /Users/eth/workspace/astral-sh/uv/bin/python -> versions/cpython@3.12.1/install/bin/python3
cpython-3.11.7-macos-aarch64-none     /Users/eth/workspace/astral-sh/uv/bin/python3.11 -> versions/cpython@3.11.7/install/bin/python3
cpython-3.10.13-macos-aarch64-none    /Users/eth/workspace/astral-sh/uv/bin/python3.10 -> versions/cpython@3.10.13/install/bin/python3
cpython-3.9.18-macos-aarch64-none     /Users/eth/workspace/astral-sh/uv/bin/python3.9 -> versions/cpython@3.9.18/install/bin/python3
cpython-3.8.18-macos-aarch64-none     /Users/eth/workspace/astral-sh/uv/bin/python3.8 -> versions/cpython@3.8.18/install/bin/python3

```


Resolves #5308 

## Test Plan

```
$ cargo run python list
```


---

_@eth3lbert reviewed on 2024-07-23 16:20_

There should be no errors here as problematic executables should have been filtered out previously.

---

_@zanieb approved on 2024-07-23 16:32_

Thanks!

---

_Comment by @zanieb on 2024-07-23 16:33_

We'll look into those CI failures — unrelated to your change.

---

_Merged by @zanieb on 2024-07-23 16:41_

---

_Closed by @zanieb on 2024-07-23 16:41_

---

_Branch deleted on 2024-07-23 16:51_

---
