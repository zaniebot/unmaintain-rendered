```yaml
number: 5005
title: "Unhide `--isolated` global argument"
type: pull_request
state: merged
author: silvanocerza
labels:
  - cli
assignees: []
merged: true
base: main
head: unhide-isolated
created_at: 2024-07-12T09:49:10Z
updated_at: 2024-07-13T08:47:36Z
url: https://github.com/astral-sh/uv/pull/5005
synced_at: 2026-01-10T13:42:52Z
```

# Unhide `--isolated` global argument

---

_Pull request opened by @silvanocerza on 2024-07-12 09:49_

## Summary

This PR makes the `--isolated` global argument visible, previously it was hidden.
Fixes #4981.

## Test Plan

I ran `cargo run -- help` to verify `--isolated` was visible and it is.
I ran clippy with `cargo clippy --workspace --all-targets --all-features --locked -- -D warnings` as CI does.

I also ran tests locally with:
```
cargo nextest run \
            --features python-patch \
            --workspace \
            --status-level skip --failure-output immediate-final --no-fail-fast -j 12 --final-status-level slow
```


---

_@zanieb approved on 2024-07-12 14:33_

Thanks!

---

_Label `cli` added by @zanieb on 2024-07-12 14:33_

---

_Merged by @zanieb on 2024-07-12 14:34_

---

_Closed by @zanieb on 2024-07-12 14:34_

---

_Branch deleted on 2024-07-13 08:47_

---
