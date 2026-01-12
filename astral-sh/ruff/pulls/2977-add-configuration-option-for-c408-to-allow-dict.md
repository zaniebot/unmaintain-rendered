```yaml
number: 2977
title: Add configuration option for C408 to allow dict calls with keyword arguments.
type: pull_request
state: merged
author: manueljacob
labels:
  - configuration
assignees: []
merged: true
base: main
head: allow_dict_calls_with_keyword_arguments
created_at: 2023-02-17T00:41:24Z
updated_at: 2023-02-19T14:47:04Z
url: https://github.com/astral-sh/ruff/pull/2977
synced_at: 2026-01-12T04:39:44Z
```

# Add configuration option for C408 to allow dict calls with keyword arguments.

---

_Pull request opened by @manueljacob on 2023-02-17 00:41_

When creating a dict with string keys, some prefer to call dict instead of writing a dict literal. For example: `dict(a=1, b=2, c=3)` instead of `{"a": 1, "b": 2, "c": 3}`.

---

_Label `configuration` added by @charliermarsh on 2023-02-19 14:43_

---

_Merged by @charliermarsh on 2023-02-19 14:47_

---

_Closed by @charliermarsh on 2023-02-19 14:47_

---
