```yaml
number: 12625
title: "Rule request: redundant type unions"
type: issue
state: closed
author: cake-monotone
labels:
  - question
assignees: []
created_at: 2024-08-02T10:15:59Z
updated_at: 2024-08-02T11:48:50Z
url: https://github.com/astral-sh/ruff/issues/12625
synced_at: 2026-01-10T11:09:54Z
```

# Rule request: redundant type unions

---

_Issue opened by @cake-monotone on 2024-08-02 10:15_

Hi! I noticed that Ruff doesn't warn about redundant type unions.

```py
c: str | str = "example"
```

While this example is straightforward, it can become confusing  when there are more than two or three types involved.

---

_Comment by @AlexWaygood on 2024-08-02 10:23_

Hiya! Have you tried enabling PYI016?

---

_Label `question` added by @AlexWaygood on 2024-08-02 11:02_

---

_Comment by @cake-monotone on 2024-08-02 11:48_

OMG 😭. Thanks so much!!
I got missed

---

_Closed by @cake-monotone on 2024-08-02 11:48_

---
