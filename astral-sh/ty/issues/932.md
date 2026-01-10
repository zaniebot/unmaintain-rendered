```yaml
number: 932
title: Plans to add auto fixes for errors?
type: issue
state: closed
author: L1ghtn1ng
labels:
  - question
assignees: []
created_at: 2025-08-04T00:24:07Z
updated_at: 2025-08-04T06:19:56Z
url: https://github.com/astral-sh/ty/issues/932
synced_at: 2026-01-10T02:06:24Z
```

# Plans to add auto fixes for errors?

---

_Issue opened by @L1ghtn1ng on 2025-08-04 00:24_

### Question

Hi All,

Apologies if I have posted this in the wrong repo, just wondering if there are going to be plans on adding fixes for type errors that can be done safely and or add the ability to add types to things where it is not typed automatically? Or is the goal to add these to ruff that would interface with ty?

### Version

_No response_

---

_Label `question` added by @L1ghtn1ng on 2025-08-04 00:24_

---

_Comment by @MichaReiser on 2025-08-04 06:19_

Yes, we plan to support auto fixes for rules where an unambiguous fix is possible in the future, see https://github.com/astral-sh/ty/issues/635. Unlike many ruff rules, typing errors are a bit trickier because it's not always clear what the proper fix is, but it should be possible for some rules. 

---

_Closed by @MichaReiser on 2025-08-04 06:19_

---
