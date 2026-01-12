```yaml
number: 12870
title: "Write requested python variant to pin file in `uv init`"
type: pull_request
state: merged
author: j178
labels: []
assignees: []
merged: true
base: main
head: init-variant
created_at: 2025-04-14T04:00:10Z
updated_at: 2025-04-15T22:32:03Z
url: https://github.com/astral-sh/uv/pull/12870
synced_at: 2026-01-12T16:10:25Z
```

# Write requested python variant to pin file in `uv init`

---

_@j178_

## Summary

Closes #12855

This PR also fixed an issue, where `python_request` was matched against `PythonVersion::Default`. Previously, if `python_request` was `3.13t`, it would match the last branch, triggering a download of the Python version if it wasn't already installed.

https://github.com/astral-sh/uv/blob/6b7f60c1eaa840c2e933a0fb056ab46f99c991a5/crates/uv/src/commands/project/init.rs#L421-L448

```console
❯ uv init -v --managed-python --python 3.13t foo
DEBUG uv 0.6.14 (a4cec56dc 2025-04-09)
DEBUG Searching for Python 3.13t in managed installations
DEBUG Searching for managed installations at `/Users/Jo/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.13.1-macos-aarch64-none`
DEBUG Found `cpython-3.13.1-macos-aarch64-none` at `/Users/Jo/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/bin/python3.13` (managed installations)
DEBUG Skipping interpreter at `/Users/Jo/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/bin/python3.13` from managed installations: does not satisfy request `3.13t`
DEBUG Skipping incompatible managed installation `cpython-3.12.8-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `pypy-3.11.11-macos-aarch64-none`
DEBUG Requested Python not found, checking for available download...
DEBUG Acquired lock for `/Users/Jo/.local/share/uv/python`
DEBUG Using request timeout of 30s
INFO Fetching requested Python...
Downloading cpython-3.13.3+freethreaded-macos-aarch64-none (49.9MiB)
DEBUG Downloading https://github.com/astral-sh/python-build-standalone/releases/download/20250409/cpython-3.13.3%2B20250409-aarch64-apple-darwin-freethreaded%2Bpgo%2Blto-full.tar.zst to temporary location: /Users/Jo/.local/share/uv/python/.temp/.tmpfoOLkE
DEBUG Extracting cpython-3.13.3%2B20250409-aarch64-apple-darwin-freethreaded%2Bpgo%2Blto-full.tar.zst
 Downloaded cpython-3.13.3+freethreaded-macos-aarch64-none
DEBUG Moving /Users/Jo/.local/share/uv/python/.temp/.tmpfoOLkE/python/install to /Users/Jo/.local/share/uv/python/cpython-3.13.3+freethreaded-macos-aarch64-none
DEBUG Released lock at `/Users/Jo/.local/share/uv/python/.lock`
DEBUG Writing Python versions to `/private/tmp/foo/.python-version`
Initialized project `foo` at `/private/tmp/foo`

❯ cat foo/.python-version
3.13
```

After this PR, uv will not try to download it:

```console
❯ uv python uninstall 3.13t
❯ cargo run -- init -v --managed-python --python 3.13t bar
DEBUG uv 0.6.14+15 (6b7f60c1e 2025-04-12)
DEBUG Writing Python versions to `/private/tmp/bar/.python-version`
Initialized project `bar` at `/private/tmp/bar`

❯ cat bar/.python_version
3.13t
```



---

_Review requested from @zanieb by @charliermarsh on 2025-04-14 12:12_

---

_Assigned to @zanieb by @charliermarsh on 2025-04-14 12:12_

---

_Comment by @charliermarsh on 2025-04-14 12:12_

Assigning @zanieb who's most familiar here.

---

_@dotysan approved on 2025-04-14 17:29_

Not tested. But looks good, thx!

---

_@zanieb approved on 2025-04-15 20:28_

Thanks!

---

_Merged by @zanieb on 2025-04-15 20:28_

---

_Closed by @zanieb on 2025-04-15 20:28_

---

_Branch deleted on 2025-04-15 22:32_

---
