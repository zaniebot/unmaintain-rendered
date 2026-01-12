```yaml
number: 7237
title: Improved string processing POC
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
created_at: 2023-09-08T08:06:20Z
updated_at: 2023-09-27T13:34:22Z
url: https://github.com/astral-sh/ruff/issues/7237
synced_at: 2026-01-12T15:54:46Z
```

# Improved string processing POC

---

_@MichaReiser_

Create a POC for #6936 to explore possible implementations and the performance implications. 

---

_Assigned to @MichaReiser by @MichaReiser on 2023-09-08 08:06_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-08 08:06_

---

_Label `formatter` added by @MichaReiser on 2023-09-08 08:08_

---

_Comment by @MichaReiser on 2023-09-08 17:27_

Learnings so far

* Black picks the preferred quote depending on the string parts that it merges. Meaning, the implicit string concatenation may have some parts that use single and other parts that use double quotes, depending on what requires the fewest escape for that specific line. Implementing this in Ruff is probably very hard, if not impossible. Ruff normalizes the entire string and picks a single quote for all parts
* I need to investigate if black applies the same formatting for triple quoted strings, and if it does, where ruff breaks these strings correctly. It could be a problem that the separator in 'flat' mode requires less space than the separator in expanded mode (one space vs triple quotes). 

---

_Removed from milestone `Formatter: Beta` by @MichaReiser on 2023-09-25 09:56_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-09-25 09:56_

---

_Closed by @MichaReiser on 2023-09-27 13:34_

---
