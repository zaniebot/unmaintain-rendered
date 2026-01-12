```yaml
number: 13080
title: "Formatter: request for a Rule Similar to ESLint’s padding-line-between-statements"
type: issue
state: open
author: ivaaahn
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-08-23T15:03:19Z
updated_at: 2024-08-25T13:54:46Z
url: https://github.com/astral-sh/ruff/issues/13080
synced_at: 2026-01-12T15:54:52Z
```

# Formatter: request for a Rule Similar to ESLint’s padding-line-between-statements

---

_@ivaaahn_

Hey, guys!

I’d like to propose a new rule for Ruff formatter, that mirrors ESLint’s padding-line-between-statements rule. This rule would add blank lines between specific statements in Python code, making it easier to read and understand.

The rule could enforce adding a blank line after an if/while/for/etc statement block. This would help to visually separate different sections of code, improving clarity.

**Before:**
```py
def process_data(value: int, data: dict) -> None:
    if data["some_key"] and value != 0:
        print("blah-blah-blah")
        ...
        print("blah-blah-blah")
    key = data["key"]
    ...
```

**After:**
```py
def process_data(value: int, data: dict) -> None:
    if data["some_key"] and value != 0:
        print("blah-blah-blah")
        ....
        print("blah-blah-blah")

    key = data["key"]
    ...
```

In the second example, the blank line after the if block makes the code structure clearer.

Is it possible to implement such a rule in Ruff? I believe this could be a useful feature for enhancing code readability in Python projects.

I’m not sure if this behavior should be enabled by default, but it would be great to have the option to configure these padding rules flexibly. For example, similar to how Ruff currently handles import groupings, it would be useful to allow developers to specify where these blank lines should or should not appear.

---

_Label `rule` added by @MichaReiser on 2024-08-23 16:13_

---

_Label `needs-decision` added by @MichaReiser on 2024-08-23 16:17_

---

_Comment by @MichaReiser on 2024-08-23 16:19_

I think this is similar to what has been asked for in https://github.com/astral-sh/ruff/issues/8632

I'm very hesitant about adding this behind a configuration flag. Doing so would require a more fundamental decision on how opinionated our formatter should be. 

I could see this as a lint rule but formatter rules are very painful to implement in the formatter and it's challenging to keep them in sync with our formatter. We don't want the formatter and linter to disagree.  We hope to make a decision on if and which formatter rules  we want to support in Ruff as part of #1774. 

---

_Comment by @ivaaahn on 2024-08-23 16:43_

Thanks for the quick response!

I understand the challenges with deciding how the formatter should work. For me, it’s not important whether this feature ends up as a formatter rule or a lint rule. I’d just be happy to see it added in any form :)

---

_Comment by @calumy on 2024-08-25 13:54_

If it is chosen that the format we should not be configurable to allow this formatting style then an alternative approach could be to implement the "CLB100" rule from the [flake8 clean block](https://github.com/cyyc1/flake8-clean-block) plugin. 

---
