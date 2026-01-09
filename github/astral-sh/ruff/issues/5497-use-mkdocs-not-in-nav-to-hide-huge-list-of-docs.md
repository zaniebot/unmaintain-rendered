---
number: 5497
title: "Use MkDocs `not_in_nav` to hide huge list of `docs/rules/` files"
type: issue
state: closed
author: MicaelJarniac
labels:
  - documentation
assignees: []
created_at: 2023-07-04T01:07:45Z
updated_at: 2023-09-19T00:01:45Z
url: https://github.com/astral-sh/ruff/issues/5497
synced_at: 2026-01-07T13:12:15-06:00
---

# Use MkDocs `not_in_nav` to hide huge list of `docs/rules/` files

---

_Issue opened by @MicaelJarniac on 2023-07-04 01:07_

[`not_in_nav`](https://github.com/mkdocs/mkdocs/blob/master/docs/user-guide/configuration.md#not_in_nav)

Would hide the following output:
```
INFO     -  The following pages exist in the docs directory, but are not included in the "nav" configuration:
              - rules/abstract-base-class-without-abstract-method.md
              - rules/airflow-variable-name-task-id-mismatch.md
              - rules/ambiguous-class-name.md
              - rules/ambiguous-function-name.md
              - ... (many other files)
```

Will be released with MkDocs 1.5, so not yet available.

---

_Referenced in [astral-sh/ruff#5498](../../astral-sh/ruff/pulls/5498.md) on 2023-07-04 01:27_

---

_Label `documentation` added by @charliermarsh on 2023-07-07 02:52_

---

_Closed by @charliermarsh on 2023-09-19 00:01_

---
