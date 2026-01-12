```yaml
number: 11406
title: "Avoid lexer usage in `PLE1300` and `PLE1307`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/avoid-lexer-in-ple
created_at: 2024-05-13T13:24:34Z
updated_at: 2024-05-13T14:50:41Z
url: https://github.com/astral-sh/ruff/pull/11406
synced_at: 2026-01-12T15:55:38Z
```

# Avoid lexer usage in `PLE1300` and `PLE1307`

---

_@dhruvmanila_

## Summary

This PR updates `PLE1300` and `PLE1307` to avoid using the lexer.

This is part of #11401 

## Test Plan

`cargo test`


---

_Label `internal` added by @dhruvmanila on 2024-05-13 13:24_

---

_@dhruvmanila reviewed on 2024-05-13 13:38_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/bad_string_format_character.rs`:95 on 2024-05-13 13:38_

Just a thought that we could provide a trait which has common methods for all `StringLiteralFlags`, `BytesLiteralFlags`, and `FStringFlags` and other methods which needs to be implemented for concrete types so that we can use the same functionality for any kind of string flags.

---

_Comment by @github-actions[bot] on 2024-05-13 13:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-05-13 13:55_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/bad_string_format_character.rs`:95 on 2024-05-13 13:55_

I think I had something like this in an early iteration of one of the string-flags PRs, but abandoned it. I think the reason I put it aside was because you need to have the trait imported for the trait methods to be available, so I felt like the methods would be hard for people implementing a new linter rule to discover. But I'm not opposed to something like that, if we think it could simplify things!

---

_@AlexWaygood reviewed on 2024-05-13 14:04_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/bad_string_format_character.rs`:95 on 2024-05-13 14:04_

I think you're right that it's unfortunate that we have to convert the `StringLiteralFlags` object to the more general `AnyStringFlags` type here in order to get the `closer_len()` method. That should really be available on `StringLiteralFlags`, `BytesLiteralFlags`, etc., as well as `AnyStringFlags`. So I agree that a trait would make sense here, especially now we're trying to reduce the number of linter rules that depend on the tokenizer

---

_@AlexWaygood approved on 2024-05-13 14:05_

---

_Merged by @charliermarsh on 2024-05-13 14:48_

---

_Closed by @charliermarsh on 2024-05-13 14:48_

---

_Branch deleted on 2024-05-13 14:48_

---

_@dhruvmanila reviewed on 2024-05-13 14:50_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/bad_string_format_character.rs`:95 on 2024-05-13 14:50_

Yeah, not a priority but something which I thought would be useful.

---
