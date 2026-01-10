```yaml
number: 1334
title: Lint when line characters are higher than maximum
type: issue
state: closed
author: DataHearth
labels:
  - question
assignees: []
created_at: 2022-12-22T14:44:19Z
updated_at: 2023-01-16T19:46:21Z
url: https://github.com/astral-sh/ruff/issues/1334
synced_at: 2026-01-10T11:09:43Z
```

# Lint when line characters are higher than maximum

---

_Issue opened by @DataHearth on 2022-12-22 14:44_

Hey!

I'm now fully using ruff in my python projects as linter and try to integrate it everywhere in my client's projects. Works like a charm and it's fast! 

I'm still using black as linter sometime because I need to format a line which exceeds the maximum line length. Unfortunately ruff doesn't not have this type of linting outside of the warning.

Is this a planned feature ? I can land a hand to implement this feature if it's interesting for the ruff project 👍.

Thanks for you awesome work!

---

_Comment by @charliermarsh on 2022-12-23 03:56_

Hey, thank you! Are you referring to the ability to reformat lines, in the event that they exceed the maximum line length?

---

_Label `question` added by @charliermarsh on 2022-12-23 03:56_

---

_Comment by @DataHearth on 2022-12-23 08:44_

exactly, here's an example with `black` formatting long lines:
```python
def func():
    print("Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.")
```

then

```python
def func():
    print(
        "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."
    )
```

The text itself has a linebreak after the first parenthesis and before the last one after linting. It is not the best example, I don't have a clean project rn to demonstrate it :). To resume, when  `line_length > max_line_length` => format the line to be shorter (multiple lines, etc).

---

_Comment by @nefrob on 2022-12-30 16:05_

+1 on this.

---

_Comment by @charliermarsh on 2022-12-30 18:31_

I do want to support this! But it requires expanding Ruff into a full autoformatter. Which is in-scope, but larger than a single Issue.

---

_Comment by @DataHearth on 2022-12-30 22:19_

@charliermarsh is there already an epic issue for this topic ? If you need help on this, I can land a hand 🙂 

---

_Comment by @charliermarsh on 2023-01-16 19:46_

Closing for now in favor of #1904!

---

_Closed by @charliermarsh on 2023-01-16 19:46_

---
