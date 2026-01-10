```yaml
number: 6203
title: "Formatter: August Goals"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - help wanted
assignees: []
created_at: 2023-07-31T16:57:46Z
updated_at: 2023-09-08T08:18:37Z
url: https://github.com/astral-sh/ruff/issues/6203
synced_at: 2026-01-10T11:09:48Z
```

# Formatter: August Goals

---

_Issue opened by @MichaReiser on 2023-07-31 16:57_

Our goal for the next four weeks / August is to make progress on some of the remaining hard problems and close the gap to Black. We're looking for contributors that help us implement the remaining syntaxes (preliminary match statement at this point) and fix black incompatibilities. Please reach out if you're interested in tackling something else. 



## Syntax support

Our goal is to support formatting of all Python syntax by the end of this iteration. This includes fixing bugs where the formatter changes semantics OR introduces syntax errors. 

* [x] #5913
* [x] #5834 
* [x] `nonlocal` statement
* [x] `global` statement
* [x]  #6064
* [x] #5894
* [x] #5941
* [x] #6196 
* [x] #6181

## Black compatibility 

Our goal is to fix some black, what we believe more complicated, Black divergences:


- [x] https://github.com/astral-sh/ruff/issues/5343
- [x] #6271
- [x] https://github.com/astral-sh/ruff/issues/6063
- [x] https://github.com/astral-sh/ruff/issues/6065
- [x] https://github.com/astral-sh/ruff/issues/6066
- [x] https://github.com/astral-sh/ruff/issues/6068

## Suppression and Pragma comments

Black supports disabling formatting for sections or a single line and implements custom logic to ensure that pragma comments (`noqa`, `type-ignore`) are kept to the syntax they applied to. Ruff needs to support these comments as well


* [x] #5336 
* [x] #6197

## Performance [Stretch]

* [x] #6016

---

_Label `formatter` added by @MichaReiser on 2023-07-31 16:58_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-07-31 16:58_

---

_Removed from milestone `Formatter: Alpha` by @MichaReiser on 2023-07-31 17:08_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-07-31 17:19_

---

_Assigned to @konstin by @MichaReiser on 2023-07-31 17:19_

---

_Label `help wanted` added by @MichaReiser on 2023-08-01 05:37_

---

_Closed by @MichaReiser on 2023-08-28 08:25_

---
