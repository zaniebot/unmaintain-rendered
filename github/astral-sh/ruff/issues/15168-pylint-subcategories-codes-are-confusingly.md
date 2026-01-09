---
number: 15168
title: "Pylint subcategories' codes are confusingly documented"
type: issue
state: closed
author: Avasam
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2024-12-28T20:19:47Z
updated_at: 2025-02-03T12:53:38Z
url: https://github.com/astral-sh/ruff/issues/15168
synced_at: 2026-01-07T13:12:16-06:00
---

# Pylint subcategories' codes are confusingly documented

---

_Issue opened by @Avasam on 2024-12-28 20:19_

https://docs.astral.sh/ruff/rules/#pylint-pl
![Image](https://github.com/user-attachments/assets/fafff0b5-037d-4a6c-bc8b-eb244a9d2ae0)

This looks like using `C`, `E`, `R` or `W` should enable/disable Pylint rules, but this is wrong as the codes are actually `PLC`, `PLE`, `PLR` and `PLW`.

This is especially vicious given that `W` and `E` are existing `pycodestyle` groups https://docs.astral.sh/ruff/rules/#pycodestyle-e-w, so config validations will pass.

Even the URL currently needs to disambiguate between pycodestyle and pylint error/warning groups https://docs.astral.sh/ruff/rules/#error-e_1 and https://docs.astral.sh/ruff/rules/#warning-w_1 (notice the `_1`)


---

_Label `documentation` added by @dhruvmanila on 2024-12-30 09:04_

---

_Comment by @MichaReiser on 2024-12-30 09:47_

Using the full error code in parentheses does make sense to me

---

_Label `help wanted` added by @MichaReiser on 2024-12-30 09:47_

---

_Comment by @VascoSch92 on 2025-01-29 09:24_

Should I open a PR to change the docs from

```text
Pylint (PL)
    Convention (C)
    Error (E)
    Refactor (R)
    Warning (W)
```

to 

```text
Pylint (PL)
    Convention (PLC)
    Error (PLE)
    Refactor (PLR)
    Warning (PLW)
```

as suggested?

---

_Comment by @MichaReiser on 2025-02-03 07:48_

@VascoSch92 that would be great!

---

_Referenced in [astral-sh/ruff#15909](../../astral-sh/ruff/pulls/15909.md) on 2025-02-03 11:56_

---

_Closed by @MichaReiser on 2025-02-03 12:53_

---
