```yaml
number: 14086
title: How to avoid automatically adding parentheses when exceeding line-length？
type: issue
state: closed
author: QJieWang
labels:
  - question
  - formatter
assignees: []
created_at: 2024-11-04T06:09:35Z
updated_at: 2024-11-04T06:55:14Z
url: https://github.com/astral-sh/ruff/issues/14086
synced_at: 2026-01-12T15:54:53Z
```

# How to avoid automatically adding parentheses when exceeding line-length？

---

_@QJieWang_

I have looked up the documentation to avoid automatically adding parentheses, but found no methods. I prefer to use `\` rather than `()` when exceeding the line length. How can I change this?

---

_Comment by @QJieWang on 2024-11-04 06:25_

https://github.com/astral-sh/ruff/issues/6271

I  do not want this feature and prefer to user `\` when change line.

---

_Comment by @MichaReiser on 2024-11-04 06:55_

I assume you're using the formatter. 

Using parentheses is non-optional because Ruff follows black's style guide. You can read about the motivation at the end of [the *How Black wraps lines](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#how-black-wraps-lines) section. 

See also https://github.com/astral-sh/ruff/issues/14025



---

_Label `question` added by @MichaReiser on 2024-11-04 06:55_

---

_Label `formatter` added by @MichaReiser on 2024-11-04 06:55_

---

_Closed by @MichaReiser on 2024-11-04 06:55_

---
