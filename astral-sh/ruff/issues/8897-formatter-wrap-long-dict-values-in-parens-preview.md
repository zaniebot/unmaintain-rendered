```yaml
number: 8897
title: "Formatter: `wrap_long_dict_values_in_parens` preview style"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
created_at: 2023-11-29T03:06:18Z
updated_at: 2024-02-12T10:01:38Z
url: https://github.com/astral-sh/ruff/issues/8897
synced_at: 2026-01-12T15:54:48Z
```

# Formatter: `wrap_long_dict_values_in_parens` preview style

---

_@MichaReiser_

Implement Black's [`wrap_long_dict_values_in_parens`](https://github.com/psf/black/pull/3440) style as a Ruff preview style. 

> **Note**: 
> This new style may not be accepted as part of Black's 2024 style guide.

```python
my_dict = {
    "a key in my dict": a_very_long_variable * and_a_very_long_function_call() * and_another_long_func() / 100000.0
}
```

**Ruff Stable**

```python
my_dict = {
    "a key in my dict": a_very_long_variable
    * and_a_very_long_function_call()
    * and_another_long_func()
    / 100000.0
}
```

**Black Preview**

```python
my_dict = {
    "a key in my dict": (
        a_very_long_variable
        * and_a_very_long_function_call()
        * and_another_long_func()
        / 100000.0
    )
}
```

This new style does not apply to subscripts, although it probably should? But this is a non goal for now and tracked separately

---

_Label `formatter` added by @MichaReiser on 2023-11-29 03:06_

---

_Label `preview` added by @MichaReiser on 2023-11-29 03:06_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-11-29 03:06_

---

_Comment by @MichaReiser on 2023-12-22 10:33_

We don't plan to implement this preview style as part of Ruff's 2024 style guide because we believe that improving the binary expression formatting solves the problem more holistically. See https://github.com/psf/black/issues/4123 for the details

---

_Removed from milestone `Formatter: Stable` by @MichaReiser on 2023-12-22 10:34_

---

_Closed by @MichaReiser on 2024-01-10 16:56_

---

_Comment by @MichaReiser on 2024-02-12 10:01_

This style did not ship as part of Black 24

---
