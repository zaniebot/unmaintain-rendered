```yaml
number: 18730
title: Formatter moves comments between f-string expr and format spec to invalid location
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - formatter
assignees: []
created_at: 2025-06-17T17:41:49Z
updated_at: 2025-06-18T12:56:16Z
url: https://github.com/astral-sh/ruff/issues/18730
synced_at: 2026-01-10T11:09:58Z
```

# Formatter moves comments between f-string expr and format spec to invalid location

---

_Issue opened by @MeGaGiGaGon on 2025-06-17 17:41_

### Summary

It looks like this case snuck though the latest f-string changes. [playground](https://play.ruff.rs/03a16098-8ac1-490e-8378-863c302d1126) this code is valid
```py
f"{1
#test
:}"
```
But gets made invalid by the formatter
```py
f"{
    1:
    # test
}"
```
(Or in the case of a multiline string, has behavior change)

### Version

ruff 0.12.0 (87f0feb21 2025-06-17) + playground

---

_Label `formatter` added by @AlexWaygood on 2025-06-17 17:48_

---

_Label `bug` added by @MichaReiser on 2025-06-18 08:14_

---

_Assigned to @MichaReiser by @MichaReiser on 2025-06-18 08:14_

---

_Comment by @MichaReiser on 2025-06-18 12:30_

Hmm, embarrassing. I must have broken this when I added "backwards compatibility". It is fixed on #18708 but I, thanks to your report, noticed another case where the comment placement wasn't ideal and fixed that too.

---

_Closed by @MichaReiser on 2025-06-18 12:56_

---
