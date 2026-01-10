```yaml
number: 14587
title: FURB157 has false negatives for non-finite float strings
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
assignees: []
created_at: 2024-11-25T16:47:41Z
updated_at: 2024-12-03T00:13:21Z
url: https://github.com/astral-sh/ruff/issues/14587
synced_at: 2026-01-10T11:09:56Z
```

# FURB157 has false negatives for non-finite float strings

---

_Issue opened by @dscorbett on 2024-11-25 16:47_

[`verbose-decimal-constructor` (FURB157)](https://docs.astral.sh/ruff/rules/verbose-decimal-constructor/) has false negatives in Ruff 0.8.0 for some non-finite float strings. It recognizes the following strings case-insensitively in expressions like `Decimal(float("inf"))`, but no others:

https://github.com/astral-sh/ruff/blob/a90e404c3f010446ab8c18b4793c78834eeb65b7/crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs#L157

`float` strips white space. For example, `Decimal(float(" nan "))` should be simplified to `Decimal(" nan ")`. It could be further normalized to `Decimal("NaN")`, but FURB157 does not currently normalize other float strings, so I think it should keep the string argument unchanged.

`float` allows both signs for all non-finite values. `Decimal(float("+inf"))`, `Decimal(float("+infinity"))`, `Decimal(float("+nan"))`, and `Decimal(float("-nan"))` should be simplified. FURB157 should not normalize these string arguments either, except for the last one. `Decimal(float("-nan"))` is equivalent to `Decimal("nan")`, not `Decimal("-nan")`, so for an argument of `"-nan"`, the sign character should be deleted.

---

_Comment by @dylwil3 on 2024-11-25 17:50_

Decimals strike again! Thanks for finding this, we'll get it fixed 😄 

---

_Label `rule` added by @dylwil3 on 2024-11-25 17:51_

---

_Label `bug` added by @dylwil3 on 2024-11-25 17:51_

---

_Closed by @dylwil3 on 2024-12-03 00:13_

---
