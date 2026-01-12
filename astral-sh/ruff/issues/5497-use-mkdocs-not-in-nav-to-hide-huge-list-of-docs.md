```yaml
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
synced_at: 2026-01-12T15:54:45Z
```

# Use MkDocs `not_in_nav` to hide huge list of `docs/rules/` files

---

_@MicaelJarniac_

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

_Label `documentation` added by @charliermarsh on 2023-07-07 02:52_

---

_Closed by @charliermarsh on 2023-09-19 00:01_

---
