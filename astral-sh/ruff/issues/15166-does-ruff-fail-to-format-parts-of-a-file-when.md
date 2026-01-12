```yaml
number: 15166
title: Does Ruff fail to format parts of a file when syntax errors are present?
type: issue
state: closed
author: work4lazy
labels:
  - question
  - formatter
assignees: []
created_at: 2024-12-28T10:44:11Z
updated_at: 2025-01-04T10:57:37Z
url: https://github.com/astral-sh/ruff/issues/15166
synced_at: 2026-01-12T15:54:54Z
```

# Does Ruff fail to format parts of a file when syntax errors are present?

---

_@work4lazy_

Hello,

I am using the Ruff extension in VSCode to automatically format Python files. However, I have noticed that when I encounter a severe syntax error while writing code, Ruff is unable to properly format any part of this file.

I would like to ask if this behavior is expected: that a severe syntax error in one place prevents Ruff from formatting all other parts of the file correctly?

Thank you for your assistance!

---

_Label `question` added by @AlexWaygood on 2024-12-28 12:05_

---

_Label `formatter` added by @AlexWaygood on 2024-12-28 12:05_

---

_Comment by @MichaReiser on 2024-12-28 22:46_

 Yes, this is the expected behavior. We may explore formatting files with syntax errors in the future but it isn't on our immediate roadmap. 

Formatting files with syntax errors comes with its own challenges because the formatter should avoid changing your code to something you didn't intend. But we could try to at least format blocks that contain no syntax errors.

---

_Closed by @MichaReiser on 2025-01-04 10:57_

---
