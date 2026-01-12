```yaml
number: 4043
title: "Avoid `PYI015` for valid default value without annotation"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/PYI015/default-value-in-assign
created_at: 2023-04-20T11:35:52Z
updated_at: 2023-04-23T10:00:29Z
url: https://github.com/astral-sh/ruff/pull/4043
synced_at: 2026-01-12T04:28:19Z
```

# Avoid `PYI015` for valid default value without annotation

---

_Pull request opened by @dhruvmanila on 2023-04-20 11:35_

## Summary

As per the original implementation in `flake8-pyi` [^1] [^2], we need to first check if the assignment value is valid.

**Valid values are:**
* Type aliases
* Type variable function calls
* Union types (`int | str`)

## Limitations:

This doesn't perform any type inference, so any variable or call would also be valid.

fixes: #4036 

[^1]: https://github.com/PyCQA/flake8-pyi/blob/4d575338389bbf7b27fc2dac05e833c818388431/pyi.py#L1021
[^2]: https://github.com/PyCQA/flake8-pyi/blob/4d575338389bbf7b27fc2dac05e833c818388431/pyi.py#L832-L840

---

_Comment by @dhruvmanila on 2023-04-20 11:58_

I'm not sure why the ecosystem checks failed to create a comment, but it can be seen here: https://github.com/charliermarsh/ruff/actions/runs/4753797743?pr=4043#summary-12893499356

---

_Merged by @charliermarsh on 2023-04-20 19:45_

---

_Closed by @charliermarsh on 2023-04-20 19:45_

---

_Comment by @charliermarsh on 2023-04-20 19:45_

LGTM!

---

_Label `bug` added by @charliermarsh on 2023-04-20 19:46_

---

_Branch deleted on 2023-04-23 10:00_

---
