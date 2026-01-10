```yaml
number: 7252
title: "Allow setting a target version for `uv self update`"
type: pull_request
state: merged
author: blueraft
labels:
  - enhancement
assignees: []
merged: true
base: main
head: update-self-target
created_at: 2024-09-10T13:06:44Z
updated_at: 2024-09-10T15:16:15Z
url: https://github.com/astral-sh/uv/pull/7252
synced_at: 2026-01-10T12:53:43Z
```

# Allow setting a target version for `uv self update`

---

_Pull request opened by @blueraft on 2024-09-10 13:06_

## Summary

Resolves #6642 

## Test Plan

```console
❯ cargo build --bin uv --features self-update
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.78s
❯ cp target/debug/uv ~/.cargo/bin
❯ uv self update 0.3.4
info: Checking for updates...
success: Upgraded uv from v0.4.8 to v0.3.4! https://github.com/astral-sh/uv/releases/tag/0.3.4
❯ uv --version
uv 0.3.4 (39f3cd2a9 2024-08-26)
❯ cp target/debug/uv ~/.cargo/bin
❯ uv self update
info: Checking for updates...
success: Upgraded uv from v0.3.4 to v0.4.8! https://github.com/astral-sh/uv/releases/tag/0.4.8
❯ uv --version
uv 0.4.8 (956cadd1a 2024-09-09)
```

---

_@charliermarsh approved on 2024-09-10 13:26_

---

_Label `enhancement` added by @charliermarsh on 2024-09-10 13:27_

---

_Comment by @charliermarsh on 2024-09-10 13:27_

This looks great, thank you.

---

_Merged by @charliermarsh on 2024-09-10 13:35_

---

_Closed by @charliermarsh on 2024-09-10 13:35_

---

_Branch deleted on 2024-09-10 15:16_

---
