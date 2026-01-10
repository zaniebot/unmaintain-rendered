```yaml
number: 3830
title: "Remove `discovery` group from CLI"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/g
created_at: 2024-05-24T18:53:27Z
updated_at: 2024-05-24T19:03:45Z
url: https://github.com/astral-sh/uv/pull/3830
synced_at: 2026-01-10T14:32:20Z
```

# Remove `discovery` group from CLI

---

_Pull request opened by @charliermarsh on 2024-05-24 18:53_

## Summary

Allows, e.g., `UV_SYSTEM_PYTHON=false uv pip install --python .venv/bin/python`.

This was intended to work after fixing https://github.com/astral-sh/uv/issues/3000, but I think I misdiagnosed the scope when closing that issue, and the linked PR there only fixed some _other_ problems around index URLs.

The only thing we really lose here is we no longer error when `--break-system-packages` is provided without `--system`, but we can enforce that elsewhere if we want.

Closes https://github.com/astral-sh/uv/issues/3829.


---

_Label `cli` added by @charliermarsh on 2024-05-24 18:53_

---

_Comment by @thejcannon on 2024-05-24 18:55_

(Not to be a commenting bystander, but are there CI tests that could encapsulate the behavior, so its demonstratable the behavior is fixed and working as expected?)

---

_@zanieb approved on 2024-05-24 18:59_

---

_Comment by @zanieb on 2024-05-24 18:59_

Test coverage could be nice.

---

_Merged by @charliermarsh on 2024-05-24 19:03_

---

_Closed by @charliermarsh on 2024-05-24 19:03_

---

_Branch deleted on 2024-05-24 19:03_

---
