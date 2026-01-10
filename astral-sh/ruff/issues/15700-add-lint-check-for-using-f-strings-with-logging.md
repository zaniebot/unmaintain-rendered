```yaml
number: 15700
title: Add lint check for using f-strings with logging methods
type: issue
state: closed
author: ktbarrett
labels:
  - question
assignees: []
created_at: 2025-01-23T20:23:32Z
updated_at: 2025-01-24T01:57:12Z
url: https://github.com/astral-sh/ruff/issues/15700
synced_at: 2026-01-10T11:09:57Z
```

# Add lint check for using f-strings with logging methods

---

_Issue opened by @ktbarrett on 2025-01-23 20:23_

### Description

Not sure if this is possible currently (may need type-aware linting), but we have noticed that many users use f-strings in calls to logging. If the logging level means that the message will not be emitted, the time spent formatting the message is a waste. Instead users should use the string formatting supported by the logging methods.

---

_Comment by @dylwil3 on 2025-01-23 23:25_

Does [logging-f-string (G004)](https://docs.astral.sh/ruff/rules/logging-f-string/#logging-f-string-g004) cover your use-case?

---

_Label `question` added by @dylwil3 on 2025-01-23 23:25_

---

_Comment by @ktbarrett on 2025-01-24 01:53_

It appears to... Sorry my Googlefu is weak.

---

_Closed by @ktbarrett on 2025-01-24 01:53_

---

_Comment by @dylwil3 on 2025-01-24 01:57_

No worries! There are lots of rules and they can be hard to search through

---
