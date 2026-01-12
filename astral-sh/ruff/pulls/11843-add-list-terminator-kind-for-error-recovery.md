```yaml
number: 11843
title: Add list terminator kind for error recovery
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - parser
assignees: []
merged: true
base: main
head: dhruv/list-terminator-kind
created_at: 2024-06-12T05:11:14Z
updated_at: 2024-06-12T08:49:56Z
url: https://github.com/astral-sh/ruff/pull/11843
synced_at: 2026-01-12T15:55:39Z
```

# Add list terminator kind for error recovery

---

_@dhruvmanila_

## Summary

This PR adds a new enum to determine the kind of terminator token i.e., is it actually terminates the list or is it used for error recovery.

This is important because the parser should take the error recovery route in case the terminator token is used for better error recovery. This will then try to re-lex the token if it's the case.

I haven't updated any reference to use this new enum as otherwise it'll update the snapshots. I plan to do that in a follow-up PR so that it's easier to reason about.

## Test plan

`cargo insta test`


---

_Label `internal` added by @dhruvmanila on 2024-06-12 05:11_

---

_Label `parser` added by @dhruvmanila on 2024-06-12 05:11_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-12 05:11_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:894 on 2024-06-12 06:22_

Nit: I would probably keep an existing `fn is_list_terminator(self, p: &Parser) -> bool` to reduce the number of call sites that need checking (I assume the kind won't be necessary in all call sites)?

---

_@MichaReiser approved on 2024-06-12 06:23_

It's not entirely clear yet how you'll be using the new kind, but I guess I'll see this with a follow up PR :)

---

_Merged by @dhruvmanila on 2024-06-12 08:33_

---

_Closed by @dhruvmanila on 2024-06-12 08:33_

---

_Branch deleted on 2024-06-12 08:33_

---

_Comment by @github-actions[bot] on 2024-06-12 08:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
