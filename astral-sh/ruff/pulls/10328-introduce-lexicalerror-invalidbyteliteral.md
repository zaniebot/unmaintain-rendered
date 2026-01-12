```yaml
number: 10328
title: "Introduce `LexicalError::InvalidByteLiteral`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/lexical-error
created_at: 2024-03-11T06:22:23Z
updated_at: 2024-03-12T08:30:23Z
url: https://github.com/astral-sh/ruff/pull/10328
synced_at: 2026-01-12T15:55:31Z
```

# Introduce `LexicalError::InvalidByteLiteral`

---

_@dhruvmanila_

## Summary

This PR introduces a new `InvalidByteLiteral` lexical error type to avoid repeating the message in multiple locations.


---

_Label `parser` added by @dhruvmanila on 2024-03-11 06:22_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-11 06:22_

---

_@VascoSch92 reviewed on 2024-03-11 07:49_

---

_Review comment by @VascoSch92 on `crates/ruff_python_parser/src/lexer.rs`:1466 on 2024-03-11 07:49_

```suggestion
                write!(f, "Bytes can only contain ASCII literal characters")
```
Nit. To be coherent with the other error messages.

---

_@dhruvmanila reviewed on 2024-03-12 08:25_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1466 on 2024-03-12 08:25_

Thanks for the suggestion. I think it's fine as not all error messages use uppercase consistently. I will be looking over the error messages at the end though and will take this into consideration.

---

_Merged by @dhruvmanila on 2024-03-12 08:30_

---

_Closed by @dhruvmanila on 2024-03-12 08:30_

---

_Branch deleted on 2024-03-12 08:30_

---
