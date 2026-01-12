```yaml
number: 11515
title: "Replace `lex_starts_at` with `Tokens` in the formatter"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - formatter
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/formatter-tokens
created_at: 2024-05-23T13:18:57Z
updated_at: 2024-05-31T04:54:04Z
url: https://github.com/astral-sh/ruff/pull/11515
synced_at: 2026-01-12T15:55:38Z
```

# Replace `lex_starts_at` with `Tokens` in the formatter

---

_@dhruvmanila_

## Summary

This PR replaces the usage of `lex_starts_at` in the formatter with the `Tokens` struct. This also updates the formatter API to take in the `Program`. Earlier, it would take the individual parts of the program but the formatter requires 3 fields from the program so we might as well pass the containing struct itself.

Part of #11401 


---

_Label `internal` added by @dhruvmanila on 2024-05-23 13:18_

---

_Label `formatter` added by @dhruvmanila on 2024-05-23 13:18_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-23 13:18_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/verbatim.rs`:731 on 2024-05-27 12:24_

Nit: Unrelated to this pr. `tokens_in_range` reads a bit strange because the word `tokens` gets repeated. I think we could just call it `in_range`, assuming that the variable or method most likely is named `tokens`.

---

_@MichaReiser approved on 2024-05-27 12:26_

---

_@dhruvmanila reviewed on 2024-05-28 04:54_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/verbatim.rs`:731 on 2024-05-28 04:54_

Yeah, I've made a note for that. I'll club this with other renames.

---

_Merged by @dhruvmanila on 2024-05-28 04:55_

---

_Closed by @dhruvmanila on 2024-05-28 04:55_

---

_Branch deleted on 2024-05-28 04:55_

---
