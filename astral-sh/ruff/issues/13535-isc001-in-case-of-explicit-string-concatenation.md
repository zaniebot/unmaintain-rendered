```yaml
number: 13535
title: ISC001 in case of explicit string concatenation
type: issue
state: closed
author: spaceby
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-09-27T10:41:08Z
updated_at: 2024-09-27T12:27:10Z
url: https://github.com/astral-sh/ruff/issues/13535
synced_at: 2026-01-12T15:54:53Z
```

# ISC001 in case of explicit string concatenation

---

_@spaceby_

```python
# ISC001 working
s_1 = 'Hello ' 'world'

# ISC001 not working
s_2 = 'Hello ' + 'world'
```
https://play.ruff.rs/019ff1c5-bc88-4ea4-b664-680efe9dc7bd

I would like ISC001 to work with explicit string concatenation as well.

---

_Comment by @MichaReiser on 2024-09-27 10:49_

Does https://docs.astral.sh/ruff/rules/explicit-string-concatenation/ work for you?

---

_Label `question` added by @MichaReiser on 2024-09-27 10:49_

---

_Comment by @spaceby on 2024-09-27 11:06_

Do you mean using ISC003 to turn explicit string concatenation into implicit, and then using ISC001 to combine them?
This will not work in the following config:
```toml
[tool.ruff.lint.flake8-implicit-str-concat]
allow-multiline = false
```

---

_Comment by @MichaReiser on 2024-09-27 11:18_

Kind of. Taking a step back. What's motivating you for this rule? Do you want to detect any form of string concatenation where both sides are constants? 

My main concern is that wich said rule, the only option to write very long string literals and keep them inside of a reasonable line length is to use a triple-quoted strings.

---

_Label `question` removed by @MichaReiser on 2024-09-27 11:18_

---

_Label `question` added by @MichaReiser on 2024-09-27 11:18_

---

_Label `rule` added by @MichaReiser on 2024-09-27 11:18_

---

_Label `needs-decision` added by @MichaReiser on 2024-09-27 11:18_

---

_Comment by @spaceby on 2024-09-27 11:30_

Ruff has a rule that detects single-line implicit string concatenation [(ISC001)](https://docs.astral.sh/ruff/rules/single-line-implicit-string-concatenation/#single-line-implicit-string-concatenation-isc001)
But ruff does not detect single-line explicit string concatenation
This is strange. This also has a negative impact on the readability of the code. 

---

_Comment by @MichaReiser on 2024-09-27 11:50_

To clarify. Do you only want to ban explicit string concatenation on a single line or in general?

---

_Comment by @spaceby on 2024-09-27 11:51_

Only on a single line. As in ISC001

---

_Comment by @MichaReiser on 2024-09-27 11:58_

I see. I'm very hesitant of adding said rule because it isn't compatible with the formatter (same as ISC001 and it now requires changes to the formatter but even then, isn't perfect). 

---

_Label `question` removed by @MichaReiser on 2024-09-27 11:58_

---

_Comment by @spaceby on 2024-09-27 12:04_

ISC001 helped me find places where I forgot to add a comma in the list of strings.
This is a useful rule.

---

_Comment by @charliermarsh on 2024-09-27 12:13_

You want to _ban_ explicit concatenation on a single line only? I don't think we're likely to support that, it's the first time we've heard that request, and implicit concatenations on a single line are a common source of bugs.

---

_Comment by @spaceby on 2024-09-27 12:19_

It seems like it would require an unreasonable amount of work.
Let's close this issue

---

_Closed by @MichaReiser on 2024-09-27 12:27_

---
