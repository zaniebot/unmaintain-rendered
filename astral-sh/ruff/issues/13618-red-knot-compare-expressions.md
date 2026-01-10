```yaml
number: 13618
title: "[red-knot] Compare expressions"
type: issue
state: closed
author: Slyces
labels:
  - ty
assignees: []
created_at: 2024-10-03T20:24:04Z
updated_at: 2024-11-07T19:51:31Z
url: https://github.com/astral-sh/ruff/issues/13618
synced_at: 2026-01-10T11:09:55Z
```

# [red-knot] Compare expressions

---

_Issue opened by @Slyces on 2024-10-03 20:24_

This is a roadmap of features and `// TODO` to mark `compare expression` as complete in astral-sh/ty#244.

- [x] Infer chained comparisons
- [x] Type pairs comparisons
  - [x] IntLiteral
  - [x] BooleanLiteral
  - [x] StringLiteral
  - [x] LiteralString
  - [x] astral-sh/ruff#13687
  - [x] astral-sh/ruff#13688
  - [x] astral-sh/ruff#13854
  - [x] astral-sh/ruff#13779
  - [x] Instances / Classes (includes most other types that are instances of `object`) https://github.com/astral-sh/ruff/issues/13872
- [x] rich comparison (`__dunder__`) algorithm
  - [x] Bug with defined method but other type that don't match the signature (Note from Alex: this should "fix itself" naturally when we start checking arguments passed to functions/methods)
  


---

_Label `red-knot` added by @dhruvmanila on 2024-10-04 03:56_

---

_Closed by @sharkdp on 2024-11-07 19:51_

---
