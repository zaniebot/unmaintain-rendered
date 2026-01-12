```yaml
number: 12856
title: Nested expression formatting
type: issue
state: open
author: MichaReiser
labels:
  - formatter
assignees: []
created_at: 2024-08-13T10:56:24Z
updated_at: 2025-04-03T10:21:43Z
url: https://github.com/astral-sh/ruff/issues/12856
synced_at: 2026-01-12T15:54:52Z
```

# Nested expression formatting

---

_@MichaReiser_

I was surprised that there's no existing issue for this. 

We decided not to implement the following Black 24 style changes:

* `parenthesize_long_type_hints,` 
* `wrap_long_dict_values_in_parens` 
* `parenthesize_conditional_expressions`


Because a) implementing this in Ruff is kind of hard (or slow) and b) it goes against the overall goal to avoid unnecessary parentheses. 

[This Black issue](https://github.com/psf/black/issues/4123) goes into more detail about our concerns. We should explore different designs that yields better sub-expression formatting without requiring optional parentheses to improve readability.

---

_Label `formatter` added by @MichaReiser on 2024-08-13 10:56_

---

_Comment by @vasily-v-ryabov on 2025-04-03 08:21_

Thanks for improving the Formatter! `ruff check` is awesome, though `ruff format` still could be a little bit more mature, but we are very positive about its future too!

Let me add some input examples here which may be useful test cases in the future. This one came from old PyLint 2.x rule W9500.
```python
some_dict = {
    'This is the first line which well fits into one line of code',
    'This is the second very long and very annoying line for the '
        'demonstration of Ruff Formatter issue with hanging indent '
        'wrapped twice in total',
    'This is the third line which well fits into one line of code',
}
```

For the next example (lower priority to us, not a deal breaker) `ruff format` removes empty line before `return` statement only at the beginning of method / function while it keeps empty line before any other `return` statement.
This one came from old PyLint 2.x rule `W9302, unnecessary-transfer-func-returns, Function returns should be transferred up. XX characters remain` (it is triggered by PyLint 2.x when empty line is removed).
```python
class Klass:
    def method1(
        self,
        other: str,
        another: str,
        one_more_argument: str,
    ) -> Optional[str]:

        return f'{other} is not {another} while {one_more_argument} happens'
```

---
