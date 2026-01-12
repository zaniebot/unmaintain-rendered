```yaml
number: 16246
title: "Ruff rule suggestion: warn for useless f-strings"
type: issue
state: open
author: Datamine
labels:
  - rule
  - type-inference
assignees: []
created_at: 2025-02-19T08:25:35Z
updated_at: 2025-02-19T10:03:11Z
url: https://github.com/astral-sh/ruff/issues/16246
synced_at: 2026-01-12T15:54:55Z
```

# Ruff rule suggestion: warn for useless f-strings

---

_@Datamine_

### Description

Occasionally I have seen f-strings used like this:
```
my_variable = "foo bar"
print(f"{my_variable}")

if f"{my_variable}" == "baz":
    return
```

In these instances, the f-string usage is improper because:

- The f-string contains only this variable, and nothing else. Using the f-string syntax is superfluous.
- If the user wanted to use a non-string variable as a string, then explicitly casting it using `str(my_variable)` would be more Pythonic.

I haven't seen a RUF rule to this effect, though I've seen a few other f-string rules. I think this would be a fine addition.

---

_Comment by @MichaReiser on 2025-02-19 10:03_

Thanks for this rule suggestion. 

As you pointed out: Whether the use of `my_variable` is unnecessary depends on if `my_variable` is already a string. Ruff's capability to detect this right now are very limited and I worry that the rule would have too many false-negatives (it couldn't flag an f-string like that if it can't determine the type of `my_variable`. 

Related https://github.com/astral-sh/ruff/issues/16103

---

_Label `rule` added by @MichaReiser on 2025-02-19 10:03_

---

_Label `type-inference` added by @MichaReiser on 2025-02-19 10:03_

---
