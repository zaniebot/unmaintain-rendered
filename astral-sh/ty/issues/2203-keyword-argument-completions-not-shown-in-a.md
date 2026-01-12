```yaml
number: 2203
title: Keyword-argument completions not shown in a decorator call without a class/function below it
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - server
  - completions
assignees: []
created_at: 2025-12-24T12:46:11Z
updated_at: 2025-12-25T15:05:48Z
url: https://github.com/astral-sh/ty/issues/2203
synced_at: 2026-01-12T15:54:26Z
```

# Keyword-argument completions not shown in a decorator call without a class/function below it

---

_@AlexWaygood_

### Summary

Consider this situation:

```py
from dataclasses import dataclass

@dataclass(fro<CURSOR>)
```

I would expect one of the completions offered here to be `frozen=`, since that's a valid keyword argument to the `dataclasses.dataclass` decorator. But this is not one of the completions offered currently:

<img width="1380" height="674" alt="Image" src="https://github.com/user-attachments/assets/c180dd34-d146-4ceb-9caa-c64b3b1ce468" />

We do include that completion if there's already a class or function definition below the decorator -- but I hadn't got to that bit yet 😆

<img width="1366" height="686" alt="Image" src="https://github.com/user-attachments/assets/b886cd36-5603-4653-9bed-ad82a4573a8b" />

As an aside -- these screenshots are from the ty playground. It looks like https://github.com/astral-sh/ruff/pull/21988 didn't work for the playground. I now see the `=` rendered as part of the completion suggestion locally in VSCode for keyword arguments, but in the playground it's still rendered as `frozen` rather than `frozen=`.

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-12-24 12:46_

---

_Label `server` added by @AlexWaygood on 2025-12-24 12:46_

---

_Label `completions` added by @AlexWaygood on 2025-12-24 12:46_

---

_Comment by @MichaReiser on 2025-12-24 15:41_

Fixing this will require changes in the parser. The issue is that we, for some reason, omit the entire `dataclass` from the parsed result instead of recovering after the `@`

---

_Closed by @MichaReiser on 2025-12-25 15:05_

---
