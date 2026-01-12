```yaml
number: 9863
title: "Docs request: linking `mypy`'s `ignore-without-code` in `PGH003`"
type: issue
state: closed
author: jamesbraza
labels:
  - documentation
assignees: []
created_at: 2024-02-06T20:32:13Z
updated_at: 2024-02-07T21:30:52Z
url: https://github.com/astral-sh/ruff/issues/9863
synced_at: 2026-01-12T15:54:49Z
```

# Docs request: linking `mypy`'s `ignore-without-code` in `PGH003`

---

_@jamesbraza_

`mypy` has an opt-in rule [`ignore-without-code`](https://mypy.readthedocs.io/en/stable/error_code_list2.html#check-that-type-ignore-include-an-error-code-ignore-without-code) that prevents blanket type ignores.

```toml
[tool.mypy]
enable_error_code = ["ignore-without-code"]
```

It would be cool to link this in the PGH003 docs: https://docs.astral.sh/ruff/rules/blanket-type-ignore/#blanket-type-ignore-pgh003

As it is an alternate configuration route to eliminate the need for `PGH003`

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-07 21:08_

---

_Label `documentation` added by @charliermarsh on 2024-02-07 21:08_

---

_Closed by @charliermarsh on 2024-02-07 21:20_

---

_Comment by @jamesbraza on 2024-02-07 21:30_

Thank you @charliermarsh ! Appreciate your continued efforts for a good user experience with `ruff` 👍 

---
