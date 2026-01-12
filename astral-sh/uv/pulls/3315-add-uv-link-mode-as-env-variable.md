```yaml
number: 3315
title: "add `UV_LINK_MODE` as env variable"
type: pull_request
state: merged
author: blueraft
labels:
  - configuration
assignees: []
merged: true
base: main
head: env-var-link-mode
created_at: 2024-04-29T17:20:14Z
updated_at: 2024-04-29T17:35:54Z
url: https://github.com/astral-sh/uv/pull/3315
synced_at: 2026-01-12T16:05:34Z
```

# add `UV_LINK_MODE` as env variable

---

_@blueraft_

Resolves https://github.com/astral-sh/uv/issues/3313

## Summary

Add a new env variable `UV_LINK_MODE` as alias for the cli argument --link-mode. 
Updated the README env variables section.

## Test Plan

Tested manually using `UV_LINK_MODE=hardlink cargo run -p uv pip install flask`.

---

_@zanieb approved on 2024-04-29 17:26_

Thanks! 

---

_Label `configuration` added by @zanieb on 2024-04-29 17:26_

---

_Merged by @zanieb on 2024-04-29 17:29_

---

_Closed by @zanieb on 2024-04-29 17:29_

---

_Branch deleted on 2024-04-29 17:35_

---
