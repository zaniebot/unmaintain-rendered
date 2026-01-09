---
number: 18255
title: Add rule to enforce blank line before control blocks (if/for/while/etc.)
type: issue
state: open
author: MathieuLoutre
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-05-22T14:16:01Z
updated_at: 2025-05-22T14:36:23Z
url: https://github.com/astral-sh/ruff/issues/18255
synced_at: 2026-01-07T13:12:16-06:00
---

# Add rule to enforce blank line before control blocks (if/for/while/etc.)

---

_Issue opened by @MathieuLoutre on 2025-05-22 14:16_

### Summary

I'd like to suggest a rule that flags when a blank line is missing above a control block (if, for, while, try, with etc.) that isn't directly after a `def`. It improves readability especially when a block begins a new logical step or decision in a function.

Input
```python
def process_data(data):
    if data not None:
        cleaned = clean(data)
        if not cleaned:
            return None
```

Output
```python
def process_data(data):
    if data not None:
        cleaned = clean(data)

        if not cleaned:
            return None
```

I'm happy to try to implement this rule and contribute a PR if this seems like a good idea. Let me know where this best belongs and if you have any preferences for configuration or rule naming.

Thanks for all the work on Ruff!

---

_Comment by @MichaReiser on 2025-05-22 14:27_

Hi. Thanks for the suggestion

This seems similar to https://github.com/astral-sh/ruff/issues/13080 except that it should enforce blank lines before an if statement. 

For now, we decided to not add any new formatting lint rules until we figured out how they'll fit into https://github.com/astral-sh/ruff/issues/1774. 

---

_Label `rule` added by @MichaReiser on 2025-05-22 14:27_

---

_Label `needs-decision` added by @MichaReiser on 2025-05-22 14:27_

---

_Comment by @MathieuLoutre on 2025-05-22 14:36_

Thanks for the super quick reply, yes https://github.com/astral-sh/ruff/issues/13080 is related (and would be great too). Effectively trying to match some of the behaviour in https://eslint.style/rules/js/padding-line-between-statements

Are you waiting to organise the existing rules before you add ones or do you want some suggestions on how to categorise that particular rule?

---
