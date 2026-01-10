```yaml
number: 16577
title: "`E501` and `W505` compatibility?"
type: issue
state: open
author: gtkacz
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-03-09T16:46:53Z
updated_at: 2025-08-19T06:37:18Z
url: https://github.com/astral-sh/ruff/issues/16577
synced_at: 2026-01-10T11:09:57Z
```

# `E501` and `W505` compatibility?

---

_Issue opened by @gtkacz on 2025-03-09 16:46_

### Question

I was just wondering, if [W505](https://docs.astral.sh/ruff/rules/doc-line-too-long/) explicitly checks for docstrings that are too long, why does [E501](https://docs.astral.sh/ruff/rules/line-too-long/) also check for that? Is there a way for me to disable E501 checking docstrings without having to manually adding `noqa` to every docstring in the project?

### Version

0.9.10

---

_Label `question` added by @gtkacz on 2025-03-09 16:46_

---

_Comment by @MichaReiser on 2025-03-10 11:04_

The idea is that `E501` acts as a global limit, while `W501` allows a stricter limit for docstrings only. 


You can disable `W501` if you want to use the same limit for docstrings as for all other code. But I suspect this doesn't work for you because you want to allow for longer docstrings?

---

_Comment by @gtkacz on 2025-03-10 14:54_

Precisely, I wanted to have longer limits for docstrings than for regular code.

---

_Comment by @gtkacz on 2025-03-24 04:20_

@MichaReiser should I turn this into a feature request or some such?

---

_Label `rule` added by @MichaReiser on 2025-03-27 20:02_

---

_Label `needs-decision` added by @MichaReiser on 2025-03-27 20:02_

---

_Label `question` removed by @MichaReiser on 2025-03-27 20:02_

---

_Comment by @1vecera on 2025-03-31 09:14_

+1 to this :) can we have a default that if docstring-code-line-length is set, E501 ignores docstrings?

---

_Comment by @vigo on 2025-06-21 18:26_

+1

---

_Comment by @gtkacz on 2025-08-18 22:48_

@MichaReiser has a decision been made on this one? Is still very relevant to me, is there anything I can do to help?

---

_Comment by @MichaReiser on 2025-08-19 06:37_

No, sorry. 

---
