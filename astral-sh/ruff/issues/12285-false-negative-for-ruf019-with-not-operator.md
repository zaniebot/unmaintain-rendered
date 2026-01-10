```yaml
number: 12285
title: "False negative for RUF019 with `not` operator"
type: issue
state: closed
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2024-07-11T00:08:47Z
updated_at: 2024-07-12T12:53:38Z
url: https://github.com/astral-sh/ruff/issues/12285
synced_at: 2026-01-10T11:09:54Z
```

# False negative for RUF019 with `not` operator

---

_Issue opened by @dscorbett on 2024-07-11 00:08_

RUF019 checks for things like `"key" in dct and dct["key"]` in boolean contexts, like an `if` condition or the argument to `bool`. It should also detect it as the operand of `not`.
```console
$ ruff --version
ruff 0.5.1
$ echo 'bool("key" in dct and dct["key"])' | ruff check --isolated --output-format=concise --select RUF019 /dev/stdin
/dev/stdin:1:6: RUF019 [*] Unnecessary key check before dictionary access
Found 1 error.
[*] 1 fixable with the `--fix` option.
$ echo 'not ("key" in dct and dct["key"])' | ruff check --isolated --output-format=concise --select RUF019 /dev/stdin
All checks passed!
```

---

_Comment by @dhruvmanila on 2024-07-11 03:45_

This seems reasonable to me.

I think we'll need to visit the operand of a unary `not` operator using the `visit_boolean_test` which sets the correct flag on the semantic model:

https://github.com/astral-sh/ruff/blob/880c31d1642a6b4d967a20f9affe9772a3722b85/crates/ruff_linter/src/checkers/ast/mod.rs#L1796-L1804

---

_Label `bug` added by @dhruvmanila on 2024-07-11 03:45_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-12 12:43_

---

_Comment by @charliermarsh on 2024-07-12 12:45_

Good call -- see: https://github.com/astral-sh/ruff/pull/12301.

---

_Closed by @charliermarsh on 2024-07-12 12:53_

---
