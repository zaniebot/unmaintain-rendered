```yaml
number: 5781
title: Remove unnecessary walrus operator parentheses
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - help wanted
assignees: []
created_at: 2023-07-15T16:10:42Z
updated_at: 2023-07-29T14:06:28Z
url: https://github.com/astral-sh/ruff/issues/5781
synced_at: 2026-01-10T11:09:48Z
```

# Remove unnecessary walrus operator parentheses

---

_Issue opened by @MichaReiser on 2023-07-15 16:10_

Our formatter currently always adds parentheses around the walrus (`a := 5`) operator in for/if/... cases

https://github.com/astral-sh/ruff/blob/3cda89ecafc24722c17d77727cde71f8d9c7df68/crates/ruff_python_formatter/src/expression/expr_named_expr.rs#L32-L41

The goal of this issue is to implement black's parentheses behavior (inside `NeedsParentheses`) and only preserve parentheses that are necessary.

https://github.com/psf/black/blob/b4dca26c7d93f930bbd5a7b552807370b60d4298/src/black/linegen.py#L1387-L1401

---

_Label `help wanted` added by @MichaReiser on 2023-07-15 16:10_

---

_Comment by @zanieb on 2023-07-15 17:00_

Related discussion suggesting we may not want to always remove them(?) https://github.com/astral-sh/ruff/discussions/4870

---

_Label `formatter` added by @MichaReiser on 2023-07-15 17:02_

---

_Comment by @MichaReiser on 2023-07-15 17:03_

> Related discussion suggesting we may not want to always remove them(?) #4870

Oh thanks. Our formatter preserves parentheses for nested expression but removes them in if/for/... headers. I should have stated this more clearly

---

_Closed by @charliermarsh on 2023-07-29 14:06_

---
