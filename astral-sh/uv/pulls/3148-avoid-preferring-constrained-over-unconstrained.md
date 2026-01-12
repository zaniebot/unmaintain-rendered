```yaml
number: 3148
title: Avoid preferring constrained over unconstrained packages
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/prio
created_at: 2024-04-19T23:16:27Z
updated_at: 2024-04-19T23:30:09Z
url: https://github.com/astral-sh/uv/pull/3148
synced_at: 2026-01-12T16:05:27Z
```

# Avoid preferring constrained over unconstrained packages

---

_@charliermarsh_

## Summary

pip prefers somewhat-constrained over unconstrained packages... but only if they're at equal depths in the tree. We don't have a way to track the latter property yet (I've added a TODO), so for now, we should remove this constraint -- it seems to be counter-productive.

I've filed https://github.com/astral-sh/uv/issues/3149 as a follow-up.

Closes https://github.com/astral-sh/uv/issues/3143

## Test Plan

- `git clone https://github.com/drivendataorg/zamba.git`
- `cat "-e .[tests]" > req.in`
- `cargo run venv && cargo run pip compile req.in --refresh -n --python-platform linux --python-version 3.8`


---

_Label `bug` added by @charliermarsh on 2024-04-19 23:16_

---

_Marked ready for review by @charliermarsh on 2024-04-19 23:16_

---

_Merged by @charliermarsh on 2024-04-19 23:30_

---

_Closed by @charliermarsh on 2024-04-19 23:30_

---

_Branch deleted on 2024-04-19 23:30_

---
