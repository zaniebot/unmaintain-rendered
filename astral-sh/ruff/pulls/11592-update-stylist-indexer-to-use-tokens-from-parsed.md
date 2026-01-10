```yaml
number: 11592
title: "Update `Stylist`, `Indexer` to use tokens from parsed output"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/indexer-stylist
created_at: 2024-05-29T01:51:00Z
updated_at: 2024-05-31T04:53:26Z
url: https://github.com/astral-sh/ruff/pull/11592
synced_at: 2026-01-10T21:56:00Z
```

# Update `Stylist`, `Indexer` to use tokens from parsed output

---

_Pull request opened by @dhruvmanila on 2024-05-29 01:51_

## Summary

This PR updates the `Stylist` and `Indexer` to get the information from `TokenFlags` instead of the owned token data. This removes the need for `Tok` completely.

Both `Stylist` and `Indexer` are now constructed using the parsed program.

This PR also removes the final few references to `Tok` and related structs. This means that clippy will now be useful and a follow-up PR will fix all the errors.

Part of #11401 


---

_Label `internal` added by @dhruvmanila on 2024-05-29 01:51_

---

_Marked ready for review by @dhruvmanila on 2024-05-29 06:05_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-29 06:05_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-05-29 06:13_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/directives.rs`:136 on 2024-05-29 06:48_

This is actually nicer :)

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/stylist.rs`:38 on 2024-05-29 06:55_

Should we just accept the tokens here instead of the entire program? It doesn't seem to need any data other than the tokens from program.

---

_Review comment by @MichaReiser on `crates/ruff_python_index/src/indexer.rs`:25 on 2024-05-29 06:56_

Same as for `Stylist`. Should this just accept the `Tokens` instead of the entire program?

---

_@MichaReiser approved on 2024-05-29 06:58_

---

_@dhruvmanila reviewed on 2024-05-29 08:36_

---

_Review comment by @dhruvmanila on `crates/ruff_python_codegen/src/stylist.rs`:38 on 2024-05-29 08:36_

Yeah, makes sense. It's probably just a side effect when I initially thought to use the AST to get the string related information. I'll update it.

---

_Merged by @dhruvmanila on 2024-05-29 08:41_

---

_Closed by @dhruvmanila on 2024-05-29 08:41_

---

_Branch deleted on 2024-05-29 08:41_

---
