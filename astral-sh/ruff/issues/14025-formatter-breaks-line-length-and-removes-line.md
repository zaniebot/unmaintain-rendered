```yaml
number: 14025
title: Formatter breaks line-length and removes line continuations 
type: issue
state: closed
author: andre-muraro-umbrage
labels:
  - formatter
  - style
assignees: []
created_at: 2024-10-31T20:42:45Z
updated_at: 2024-11-01T06:49:12Z
url: https://github.com/astral-sh/ruff/issues/14025
synced_at: 2026-01-12T15:54:53Z
```

# Formatter breaks line-length and removes line continuations 

---

_@andre-muraro-umbrage_

When applying `format` , it removes line continuations resulting in E501 violations: 

Before:
```python
some_dict = {
    "some_really_long_key_name": some_ridiculously_even_impossibly_long_variable_name. \
        THIS_IS_A_RIDICULOUSLY_LONG_PROPERTY_NAME
}
```
After:
```python
some_dict = {
    "some_really_long_key_name": some_ridiculously_even_impossibly_long_variable_name.THIS_IS_A_RIDICULOUSLY_LONG_PROPERTY_NAME
}
# ˆˆˆ breaks E501
```

my local version `0.6.9`

POSSIBLY related 
[#11820](https://github.com/astral-sh/ruff/issues/11820)
[#10272](https://github.com/astral-sh/ruff/issues/10272)

[playground](https://play.ruff.rs/c4bc5f99-8c09-4836-ba58-10f6c61fa7bf)
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


---

_Label `formatter` added by @AlexWaygood on 2024-10-31 22:03_

---

_Comment by @charliermarsh on 2024-10-31 23:37_

This is an intentional formatting decision -- continuations are always removed.

---

_Label `style` added by @MichaReiser on 2024-11-01 06:48_

---

_Comment by @MichaReiser on 2024-11-01 06:48_

You can parenthesize the value if you want to shorten the line a bit:

```py
some_dict = {
    "some_really_long_key_name": (
        some_ridiculously_even_impossibly_long_variable_name.THIS_IS_A_RIDICULOUSLY_LONG_PROPERTY_NAME
    )
}
# ˆˆˆ breaks E501
```

---

_Closed by @MichaReiser on 2024-11-01 06:49_

---
