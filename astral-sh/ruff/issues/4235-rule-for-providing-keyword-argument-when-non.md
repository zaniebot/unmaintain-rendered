```yaml
number: 4235
title: (🎁) Rule for providing keyword argument when non-meaningful literal is passed as positional argument
type: issue
state: open
author: KotlinIsland
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-05-05T04:46:04Z
updated_at: 2023-07-10T01:29:31Z
url: https://github.com/astral-sh/ruff/issues/4235
synced_at: 2026-01-12T15:54:44Z
```

# (🎁) Rule for providing keyword argument when non-meaningful literal is passed as positional argument

---

_@KotlinIsland_

For the following call:
```py
a = textwrap.TextWrapper(70, "", "", True, True, False, True, True, False, 4)
```
It's very hard to understand what the code is doing. Instead you'd want to have keyword arguments:

```py
a = textwrap.TextWrapper(
    width=70,
    initial_indent="",
    subsequent_indent="",
    expand_tabs=True,
    replace_whitespace=True,
    fix_sentence_endings=False,
    break_long_words=True,
    drop_whitespace=True,
    break_on_hyphens=False,
    tabsize=4
)           
```

Inspired by: https://github.com/pylint-dev/pylint/issues/385

---

_Label `rule` added by @charliermarsh on 2023-05-05 18:02_

---

_Comment by @charliermarsh on 2023-05-05 18:02_

Reminiscent of the `flake8-boolean-trap` rules.

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:29_

---
