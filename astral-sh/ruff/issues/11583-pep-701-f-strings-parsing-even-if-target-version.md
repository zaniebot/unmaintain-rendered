```yaml
number: 11583
title: "PEP 701 f-strings parsing even if `--target-version` < `py312`"
type: issue
state: closed
author: miccoli
labels: []
assignees: []
created_at: 2024-05-28T14:58:06Z
updated_at: 2024-05-28T16:48:02Z
url: https://github.com/astral-sh/ruff/issues/11583
synced_at: 2026-01-12T15:54:51Z
```

# PEP 701 f-strings parsing even if `--target-version` < `py312`

---

_@miccoli_

In ruff version 0.4.5 f-strings are parsed according to [PEP701](https://peps.python.org/pep-0701/) even if the target python version is `< 3.12`:
```bash
echo 's = f"{"0"}"' | ruff check --isolated --target-version py311 -
All checks passed!
```
I would expect an E999 error unless the target version is at least 3.12:
```bash
echo 's = f"{"0"}"' | python3.11 -
  File "<stdin>", line 1
    s = f"{"0"}"
            ^
SyntaxError: f-string: expecting '}'
```


---

_Comment by @MichaReiser on 2024-05-28 16:05_

Thanks for opening this issue. Yes, we should emit a diagnostic if a program uses unsupported syntax. There's a tracking issue that you can follow that lists the different python-version specific syntaxes that Ruff should warn about. https://github.com/astral-sh/ruff/issues/6591

---

_Closed by @MichaReiser on 2024-05-28 16:05_

---

_Comment by @miccoli on 2024-05-28 16:48_

Sorry for having missed the linked issue: I searched for PEP701 (no spaces) and GH wasn't able to locate the relevant issue. Somehow the GH search is more strict than I assumed!

BTW thanks for the excellent tool and support.

---
