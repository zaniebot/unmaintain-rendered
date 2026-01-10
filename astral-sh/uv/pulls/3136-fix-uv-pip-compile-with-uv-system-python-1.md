```yaml
number: 3136
title: "Fix `uv pip compile` with `UV_SYSTEM_PYTHON=1`"
type: pull_request
state: merged
author: jfcherng
labels: []
assignees: []
merged: true
base: main
head: fix/uv-pip-compile-bool
created_at: 2024-04-19T04:27:07Z
updated_at: 2024-04-20T13:54:41Z
url: https://github.com/astral-sh/uv/pull/3136
synced_at: 2026-01-10T14:43:31Z
```

# Fix `uv pip compile` with `UV_SYSTEM_PYTHON=1`

---

_Pull request opened by @jfcherng on 2024-04-19 04:27_

## Summary

Following up 

- https://github.com/astral-sh/uv/pull/3113
- https://github.com/astral-sh/uv/pull/3115

It looks like `uv pip compile` command with `UV_SYSTEM_PYTHON=1` is missed because these two PRs are close in time. And thus resulting in


```bash
$ uv --version
uv 0.1.34 (9259eceeb 2024-04-19)

$ UV_SYSTEM_PYTHON=1 uv pip compile --upgrade requirements.in -o requirements.txt
error: invalid value '1' for '--system'
  [possible values: true, false]

For more information, try '--help'.
```

---

_@charliermarsh approved on 2024-04-19 12:48_

Thx

---

_Merged by @charliermarsh on 2024-04-19 12:48_

---

_Closed by @charliermarsh on 2024-04-19 12:48_

---

_Branch deleted on 2024-04-19 12:56_

---
