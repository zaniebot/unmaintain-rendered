---
number: 17973
title: How to disable alphabetical import sort
type: issue
state: closed
author: juanzolotoochin
labels:
  - question
assignees: []
created_at: 2025-05-08T23:03:50Z
updated_at: 2025-05-28T07:52:43Z
url: https://github.com/astral-sh/ruff/issues/17973
synced_at: 2026-01-07T13:12:16-06:00
---

# How to disable alphabetical import sort

---

_Issue opened by @juanzolotoochin on 2025-05-08 23:03_

### Question

I'm dealing with a codebase that has circular dependencies and reordering the first party import statements breaks everything.

How can I make ruff sort imports by type (first vs third party, etc) and keep the grouping but not sort first party alphabetically?

### Version

_No response_

---

_Label `question` added by @juanzolotoochin on 2025-05-08 23:03_

---

_Comment by @MichaReiser on 2025-05-09 06:03_

Can you tell me more why sorting imports alphabetical breaks everything? That sorting shouldn't rely on any cross module (or external) information

Disabling sorting within groups isn't supported, see [isort settings](https://docs.astral.sh/ruff/settings/#lintisort). You can change the sorting to be by-length instead but that isn't what you seem to be looking for.

---

_Comment by @ntBre on 2025-05-09 13:05_

I've run into this before in Python code too. Sometimes with circular imports you can break the cycle (or at least avoid the error) by running imports in the right order. I was still using isort directly at the time, but I would hack around it with something like

```python
import b
if True:
    import a
import c
```

but one of the [isort action comments](https://pycqa.github.io/isort/docs/configuration/action_comments.html) might have worked too.

---

_Closed by @MichaReiser on 2025-05-28 07:52_

---
