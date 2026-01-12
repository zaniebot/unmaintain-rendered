```yaml
number: 13807
title: "[verbose-decimal-constructor (FURB157)] doesn't trigger on `Decimal(\"1_000\")`"
type: issue
state: closed
author: monosans
labels:
  - bug
  - help wanted
  - preview
assignees: []
created_at: 2024-10-18T09:24:40Z
updated_at: 2025-09-25T08:26:46Z
url: https://github.com/astral-sh/ruff/issues/13807
synced_at: 2026-01-12T15:54:53Z
```

# [verbose-decimal-constructor (FURB157)] doesn't trigger on `Decimal("1_000")`

---

_@monosans_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

```python
Decimal(1000)  # OK
Decimal(1_000)  # OK
Decimal("1000")  # Triggers FURB157
Decimal("1_000")  # Should trigger FURB157 and become Decimal(1_000), but it doesn't
```

ruff v0.7.0

---

_Comment by @AlexWaygood on 2024-10-18 12:32_

It seems like the parsing we do here is too naive: https://github.com/astral-sh/ruff/blob/4ecfe95295eb203ba771f344f52c8e61527a4c05/crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs#L77-L98

---

_Label `bug` added by @AlexWaygood on 2024-10-18 12:32_

---

_Label `help wanted` added by @AlexWaygood on 2024-10-18 12:32_

---

_Label `preview` added by @AlexWaygood on 2024-10-18 12:33_

---

_Comment by @AlexWaygood on 2024-10-18 12:42_

If there are more edge cases that we don't handle, it might be worth just using `ruff_python_parser::parse_expression` to determine if the contents of the string can be parsed as an `int` literal, rather than reimplementing all the edge cases in the linter. If this is the only edge case we don't currently handle, however, it may be best to avoid doing that and just reimplement the logic in the linter -- using the `ruff_python_parser` function might be comparatively quite expensive.

---

_Comment by @dscorbett on 2024-10-18 15:02_

`Decimal` accepts more integer-valued strings than are valid as `int` literals. For example, `Decimal("1__2")` and `Decimal("_1_")` and `Decimal("١٢٣")` are valid. Should FURB157 trigger on those? If so, `ruff_python_parser::parse_expression` would not be enough.

---

_Comment by @dylwil3 on 2024-10-19 03:48_

Looks like this is the official regex: 

https://github.com/python/cpython/blob/322f14eeff9e3b5853eaac3233f7580ca0214cf8/Lib/_pydecimal.py#L6059-L6077

I can try to take this sometime this weekend (but if I don't, others should feel free to go for it!)

---

_Assigned to @dylwil3 by @dylwil3 on 2024-11-03 19:40_

---

_Closed by @dylwil3 on 2024-11-05 19:33_

---

_Comment by @m-aciek on 2025-09-25 08:26_

The fix in latest ruff (v0.13.1) converts `Decimal("1_000")` to `Decimal(1000)` instead of to `Decimal(1_000)`.

It becomes more problematic for bigger values, where thousand formatting really helps to read a number, like `Decimal(15_000_000)`.

Can the issue be reopened?

---
