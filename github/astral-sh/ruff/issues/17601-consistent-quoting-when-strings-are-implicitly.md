---
number: 17601
title: Consistent quoting when strings are implicitly concatenated
type: issue
state: open
author: pekkaklarck
labels:
  - formatter
  - style
assignees: []
created_at: 2025-04-24T09:24:54Z
updated_at: 2025-04-26T11:56:28Z
url: https://github.com/astral-sh/ruff/issues/17601
synced_at: 2026-01-07T13:12:16-06:00
---

# Consistent quoting when strings are implicitly concatenated

---

_Issue opened by @pekkaklarck on 2025-04-24 09:24_

I believe Ruff should use consistent quoting when long strings are constructed using implicit concatenation. That obviously isn't possible if fragments use both single and double quotes as literal values, but that's not the common case.

For example, if I have code like
```python
variable = 'value'
print(
    f"I'm trying to create a somewhat reasonable example here. This is a '{variable}'.\n"
    f"Here we have the second line."
)
```
and format it with `quote-style = 'single'`, I get this result:
```python
variable = 'value'
print(
    f"I'm trying to create a somewhat reasonable example here. This is a '{variable}'.\n"
    f'Here we have the second line.'
)
```
At least for me the formatted string with `f"` and `f'` prefixes looks odd. I'd prefer `f"` being used consistently even though I explicitly requested single quotes.

---

_Label `formatter` added by @MichaReiser on 2025-04-24 09:30_

---

_Label `style` added by @MichaReiser on 2025-04-24 09:30_

---

_Comment by @dhruvmanila on 2025-04-24 12:29_

This is occurring because the first f-string has multiple single quotes inside the literal part of the f-string (e.g., `I'm`) and the formatter will prefer to avoid using escapes. This means it won't convert it to `'I\'m'` and instead use `"I'm"`.

The formatter cannot change the quotes in the literal parts of the f-strings because that would be a semantic change as it would change the content of the f-strings.

---

_Comment by @pekkaklarck on 2025-04-26 11:56_

I understand why the first fragment with single quotes in the string itself is surrounded with double quotes even though I used `quote-style = 'single'`. I also prefer it that way. In my opinion it would be more logical to surround _also_ latter fragments with double quotes regardless do they contain single quotes or no (unless they contain double quotes). The quoting style changing within the same implicitly concatenated string feels strange for me.

There's a similar situation when implicitly concatenating f-strings. At least I prefer all fragments to have the f-prefix regardless do they contain variables or not:
```python
print(
    f'Hello, {name}! '
    f'Nice to see you! ',
    f'Hold my {beverage}.'
)
```
vs
```python
print(
    f'Hello, {name}! '
    'Nice to see you! ',
    f'Hold my {beverage}.'
)
```


I was kind of shocked when I noticed that [Black is planning to change this](https://black.readthedocs.io/en/stable/the_black_code_style/future_style.html#improved-string-processing). If that happens, I hope Ruff either keeps the old behavior or makes it configurable, but that's a topic for a separate issue.

---
