```yaml
number: 13305
title: "Suggest `uv self update` if required version is newer"
type: pull_request
state: merged
author: blueraft
labels:
  - enhancement
assignees: []
merged: true
base: main
head: suggest-self-update
created_at: 2025-05-05T20:40:43Z
updated_at: 2025-05-08T00:09:29Z
url: https://github.com/astral-sh/uv/pull/13305
synced_at: 2026-01-12T16:10:38Z
```

# Suggest `uv self update` if required version is newer

---

_@blueraft_

## Summary

Closes #13253 

## Test Plan

```sh
❯ cat pyproject.toml | rg required
required-version = ">=0.7.3, <0.8"
❯ cargo run -q --features self-update --manifest-path ~/uv/Cargo.toml add black
error: Required uv version `>=0.7.3, <0.8` does not match the running version `0.7.2`.
hint: Update `uv` by running `uv self update`.
❯ cat pyproject.toml | rg required
required-version = ">=0.7.3"
❯ cargo run -q --features self-update --manifest-path ~/uv/Cargo.toml add black
error: Required uv version `>=0.7.3` does not match the running version `0.7.2`. 
hint: Update `uv` by running `uv self update`.
❯ cat pyproject.toml | rg required
required-version = "<0.7"
❯ cargo run -q --features self-update --manifest-path ~/uv/Cargo.toml add black
error: Required uv version `<0.7` does not match the running version `0.7.2`.
❯ cat pyproject.toml | rg required
required-version = ">=0.4,<0.7"
❯ cargo run -q --features self-update --manifest-path ~/uv/Cargo.toml add black
error: Required uv version `>=0.4, <0.7` does not match the running version `0.7.2`.
```


---

_Comment by @konstin on 2025-05-05 20:52_

This PR is trying to fix the same issue #13258, though I haven't review either one yet. CC @zanieb 

---

_Comment by @blueraft on 2025-05-05 20:54_

Oops sorry! The implementation is slightly different, so I'll just keep this one open too

---

_@charliermarsh approved on 2025-05-07 23:57_

---

_Label `enhancement` added by @charliermarsh on 2025-05-07 23:57_

---

_Comment by @charliermarsh on 2025-05-07 23:57_

Thanks! I just tweaked to check for singletons since that's pretty common.

---

_Merged by @charliermarsh on 2025-05-08 00:09_

---

_Closed by @charliermarsh on 2025-05-08 00:09_

---
