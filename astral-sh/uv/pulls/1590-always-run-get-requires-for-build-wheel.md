```yaml
number: 1590
title: "Always run `get_requires_for_build_wheel`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/req
created_at: 2024-02-17T14:18:13Z
updated_at: 2024-02-17T14:24:59Z
url: https://github.com/astral-sh/uv/pull/1590
synced_at: 2026-01-12T16:04:40Z
```

# Always run `get_requires_for_build_wheel`

---

_@charliermarsh_

## Summary

I want to revisit this as I think it's still skippable in some cases, but for now, let's be more conservative.

Closes https://github.com/astral-sh/uv/issues/1582.

## Test Plan

Cloned `https://github.com/OCA/mis-builder/blob/3aea4235697bac0f74d446e610e2b934b0994e06/setup/mis_builder/setup.py#L4`, and ran `cargo run pip install -e mis_builder`.


---

_Label `bug` added by @charliermarsh on 2024-02-17 14:18_

---

_Comment by @charliermarsh on 2024-02-17 14:23_

I'll follow-up with more test coverage and some other heuristics, but want to get this out since it's a correctness thing.

---

_Merged by @charliermarsh on 2024-02-17 14:24_

---

_Closed by @charliermarsh on 2024-02-17 14:24_

---

_Branch deleted on 2024-02-17 14:24_

---
