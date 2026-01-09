---
number: 8417
title: "[doc] isort deviation: action comments in docstrings not respected"
type: issue
state: closed
author: Avasam
labels:
  - documentation
assignees: []
created_at: 2023-11-01T18:17:44Z
updated_at: 2023-11-02T03:22:35Z
url: https://github.com/astral-sh/ruff/issues/8417
synced_at: 2026-01-07T13:12:15-06:00
---

# [doc] isort deviation: action comments in docstrings not respected

---

_Issue opened by @Avasam on 2023-11-01 18:17_

This was noticed here: https://github.com/python/typeshed/pull/10939

I think the deviation makes sense, but it should probably at least be documented in https://docs.astral.sh/ruff/linter/#action-comments

---

_Comment by @charliermarsh on 2023-11-02 02:11_

Ahh I see -- so the `isort:skip_file` is in the docstring at the top of the file. Got it. I will document.

---

_Label `documentation` added by @charliermarsh on 2023-11-02 02:11_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-02 02:12_

---

_Referenced in [astral-sh/ruff#8432](../../astral-sh/ruff/pulls/8432.md) on 2023-11-02 03:15_

---

_Closed by @charliermarsh on 2023-11-02 03:22_

---
