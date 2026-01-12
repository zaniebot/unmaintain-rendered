```yaml
number: 448
title: invalid-exception-caught false negative when exception is not captured
type: issue
state: closed
author: sk-
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-05-19T12:18:23Z
updated_at: 2025-05-19T22:13:35Z
url: https://github.com/astral-sh/ty/issues/448
synced_at: 2026-01-12T15:54:23Z
```

# invalid-exception-caught false negative when exception is not captured

---

_@sk-_

### Summary

`invalid-exception-caught` seems to be triggered only when the exception is captured as seen in the code below (see [playground](https://play.ty.dev/21cde03f-35c3-4461-a651-ba679664c859))

```python
try:
    {}.get("foo")
except int:
    pass

try:
    {}.get("foo")
except int as err:
    pass
```

where only the second try/catch generates the error

```
Cannot catch object of type `<class 'int'>` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses) (invalid-exception-caught) [Ln 8, Col 8]
```

### Version

ty 0.0.1-alpha.5 (3b726d87a 2025-05-17) and playground

---

_Label `bug` added by @AlexWaygood on 2025-05-19 12:19_

---

_Label `help wanted` added by @sharkdp on 2025-05-19 14:46_

---

_Comment by @adamaaronson on 2025-05-19 14:58_

I'm interested in taking this!

---

_Assigned to @adamaaronson by @AlexWaygood on 2025-05-19 14:58_

---

_Closed by @AlexWaygood on 2025-05-19 22:13_

---
