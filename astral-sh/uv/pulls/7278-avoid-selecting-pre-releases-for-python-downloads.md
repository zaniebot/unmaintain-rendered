```yaml
number: 7278
title: Avoid selecting pre-releases for Python downloads without a version request
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-313-install
created_at: 2024-09-10T22:01:04Z
updated_at: 2024-09-10T22:20:19Z
url: https://github.com/astral-sh/uv/pull/7278
synced_at: 2026-01-12T16:07:46Z
```

# Avoid selecting pre-releases for Python downloads without a version request

---

_@zanieb_

Following #7263 the 3.13.0rc2 releases are at the top of the download list but we should not select them unless 3.13 is actually requested.

Prior to this, `uv python install` would install `3.13.0rc2`. 

```
❯ cargo run -- python install --no-config
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/uv python install --no-config`
Searching for Python installations
Installed Python 3.12.6 in 1.33s
 + cpython-3.12.6-macos-aarch64-none
```

```
❯ cargo run -- python install --no-config 3.13
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/uv python install --no-config 3.13`
Searching for Python versions matching: Python 3.13
Installed Python 3.13.0rc2 in 1.18s
 + cpython-3.13.0rc2-macos-aarch64-none
```


---

_Comment by @zanieb on 2024-09-10 22:12_

This breaks `uv python uninstall --all` because it uses `PythonDownloadRequest` which needs to match here. Tweaking..

---

_Comment by @zanieb on 2024-09-10 22:14_

Okay all better 

```
❯ cargo run -- python install --no-config
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/uv python install --no-config`
Searching for Python installations
Installed Python 3.12.6 in 1.20s
 + cpython-3.12.6-macos-aarch64-none
❯ cargo run -- python install --no-config 3.13
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/uv python install --no-config 3.13`
Searching for Python versions matching: Python 3.13
Installed Python 3.13.0rc2 in 1.25s
 + cpython-3.13.0rc2-macos-aarch64-none
❯ cargo run -- python uninstall --all
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/uv python uninstall --all`
Searching for Python installations
Uninstalled 2 versions in 82ms
 - cpython-3.12.6-macos-aarch64-none
 - cpython-3.13.0rc2-macos-aarch64-none
❯ cargo run -- python install --no-config 3.13
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/uv python install --no-config 3.13`
Searching for Python versions matching: Python 3.13
Installed Python 3.13.0rc2 in 1.24s
 + cpython-3.13.0rc2-macos-aarch64-none
❯ cargo run -- python install --no-config
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/uv python install --no-config`
Searching for Python installations
Found: cpython-3.13.0rc2-macos-aarch64-none
Python is already available. Use `uv python install <request>` to install a specific version.
```

---

_Label `bug` added by @zanieb on 2024-09-10 22:14_

---

_Comment by @zanieb on 2024-09-10 22:15_

Cancelled the #7274 release for this — will re-run after merge. Users aren't impacted by this bug since it's unreleased.

---

_@charliermarsh approved on 2024-09-10 22:18_

---

_Merged by @zanieb on 2024-09-10 22:20_

---

_Closed by @zanieb on 2024-09-10 22:20_

---

_Branch deleted on 2024-09-10 22:20_

---
