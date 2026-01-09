---
number: 21596
title: Rule for certain hanging indents
type: issue
state: closed
author: AckslD
labels: []
assignees: []
created_at: 2025-11-23T19:45:37Z
updated_at: 2025-11-24T09:00:32Z
url: https://github.com/astral-sh/ruff/issues/21596
synced_at: 2026-01-07T13:12:16-06:00
---

# Rule for certain hanging indents

---

_Issue opened by @AckslD on 2025-11-23 19:45_

### Summary

It would be great to have the ability to enable rules which dissallow indentation based on for example a function name instead of eg 4 spaces. Ie to disallow:

```python
def my_really_long_function_name(a, 
                                 b)
```
but prefer
```python
def my_really_long_function_name(
    a, 
    b,
)
```

Something like https://github.com/deniskrumko/flake8-hangover.

I wanted to first check if there could be an interest for this. If so I can try to make it a bit more precise and also happy to try to work on the actual implementation once agreed upon.

---

_Comment by @MichaReiser on 2025-11-24 08:55_

Hi @AckslD 

I'll merge this into https://github.com/astral-sh/ruff/issues/9216 as adding an option outlined in https://github.com/astral-sh/ruff/issues/9216#issuecomment-2904749624 would do exactly this

---

_Closed by @MichaReiser on 2025-11-24 08:55_

---

_Comment by @AckslD on 2025-11-24 09:00_

Great, thanks @MichaReiser !

---
