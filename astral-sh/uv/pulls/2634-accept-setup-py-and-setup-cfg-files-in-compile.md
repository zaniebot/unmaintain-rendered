```yaml
number: 2634
title: "Accept `setup.py` and `setup.cfg` files in compile"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - compatibility
assignees: []
merged: true
base: main
head: charlie/setup-iii
created_at: 2024-03-23T02:35:34Z
updated_at: 2024-03-25T20:39:37Z
url: https://github.com/astral-sh/uv/pull/2634
synced_at: 2026-01-12T16:05:08Z
```

# Accept `setup.py` and `setup.cfg` files in compile

---

_@charliermarsh_

## Summary

This costs us ~nothing now that we support using PEP 517 hooks for `pyproject.toml`.

## Test Plan

`cargo test`


---

_Label `enhancement` added by @charliermarsh on 2024-03-23 02:35_

---

_Label `compatibility` added by @charliermarsh on 2024-03-23 02:35_

---

_Marked ready for review by @charliermarsh on 2024-03-23 02:35_

---

_@zanieb approved on 2024-03-23 02:37_

---

_@zanieb approved on 2024-03-25 19:43_

Should we add an example to the README alongside the other sources?

---

_Merged by @charliermarsh on 2024-03-25 20:39_

---

_Closed by @charliermarsh on 2024-03-25 20:39_

---

_Branch deleted on 2024-03-25 20:39_

---
