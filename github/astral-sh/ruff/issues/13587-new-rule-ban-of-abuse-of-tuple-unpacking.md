---
number: 13587
title: New rule - ban of abuse of tuple unpacking
type: issue
state: closed
author: spaceby
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-10-01T11:11:02Z
updated_at: 2024-10-07T07:01:51Z
url: https://github.com/astral-sh/ruff/issues/13587
synced_at: 2026-01-07T13:12:15-06:00
---

# New rule - ban of abuse of tuple unpacking

---

_Issue opened by @spaceby on 2024-10-01 11:11_

In the project I work on, I sometimes find this
```python
def the_name_is_too_long_for_two_of_them_to_fit_on_one_line(n):
    return n * 2


a_variable = 1
b_variable = 2

a_coefficient, b_coefficient = the_name_is_too_long_for_two_of_them_to_fit_on_one_line(a_variable), \
    the_name_is_too_long_for_two_of_them_to_fit_on_one_line(b_variable)
```

I found something like this once
```python
def the_name_is_too_long_for_two_of_them_to_fit_on_one_line(n):
    return n * 2


a_variable = 1
b_variable = 2

a_coefficient, b_coefficient = (the_name_is_too_long_for_two_of_them_to_fit_on_one_line(item)
                                for item in (a_variable, b_variable))
```
In the first example, unpacking simply doesn't make sense.
In the second example, the code turned out shorter. But the price was understanding which variables lead to which result.

It's better to just do it like this:
```python
def the_name_is_too_long_for_two_of_them_to_fit_on_one_line(n):
    return n * 2


a_variable = 1
b_variable = 2

a_coefficient = the_name_is_too_long_for_two_of_them_to_fit_on_one_line(a_variable)
b_coefficient = the_name_is_too_long_for_two_of_them_to_fit_on_one_line(b_variable)
```

---

_Label `rule` added by @AlexWaygood on 2024-10-01 15:50_

---

_Label `needs-decision` added by @AlexWaygood on 2024-10-01 15:50_

---

_Comment by @MichaReiser on 2024-10-07 07:01_

I merge this with https://github.com/astral-sh/ruff/issues/11317 that I understand asks for the same rule.

---

_Closed by @MichaReiser on 2024-10-07 07:01_

---
