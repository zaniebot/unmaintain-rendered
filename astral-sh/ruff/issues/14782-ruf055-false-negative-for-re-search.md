```yaml
number: 14782
title: "RUF055: false negative for `re.search(...)`"
type: issue
state: open
author: trim21
labels:
  - type-inference
assignees: []
created_at: 2024-12-05T07:52:44Z
updated_at: 2024-12-06T17:34:07Z
url: https://github.com/astral-sh/ruff/issues/14782
synced_at: 2026-01-12T15:54:54Z
```

# RUF055: false negative for `re.search(...)`

---

_@trim21_

```python
import re


def test_cost_repeated_merge(errors):
    assert any(re.search("Duplicate merge", error.message) for error in errors)

```

```text
$ ruff check d.py --select RUF055 --preview
All checks passed!

$ ruff --version
ruff 0.8.1
```

plarground: https://play.ruff.rs/9db437a8-cdfd-457c-a964-09fd788b9970

---

_Comment by @MichaReiser on 2024-12-05 08:02_

The issue here is that Ruff doesn't detect that `re.search` is used in a boolean position. I don't think it's worth special casing `any` (and many other functions that take boolean arguments)

---

_Label `type-inference` added by @MichaReiser on 2024-12-05 08:02_

---

_Comment by @trim21 on 2024-12-06 17:21_

> The issue here is that Ruff doesn't detect that `re.search` is used in a boolean position. I don't think it's worth special casing `any` (and many other functions that take boolean arguments)

Wouldn't it make more sence to rewrite `re.search("Duplicate merge", error.message)` to `"Duplicate merge" in error.message` to fit in boolean type?

---

_Comment by @MichaReiser on 2024-12-06 17:29_

It's not about the rewrite. It's that Ruff doesn't think it's safe the change the `search` call because it doesn't know that the `re.search` result is used in a boolean position - the code only tests if the result is true or false. 

---

_Comment by @trim21 on 2024-12-06 17:32_

OK， I misunderstand. I thought that ruff know it's used as bool and decide not to report RUF055 on it

---
