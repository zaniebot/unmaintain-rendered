```yaml
number: 11356
title: "Use `u64` instead of `i64` in Int type"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/i
created_at: 2024-05-10T01:18:06Z
updated_at: 2024-05-10T13:44:54Z
url: https://github.com/astral-sh/ruff/pull/11356
synced_at: 2026-01-12T15:55:37Z
```

# Use `u64` instead of `i64` in Int type

---

_@charliermarsh_

## Summary

I believe the value here is always unsigned, since we represent `-42` as a unary operator on `42`.


---

_Label `internal` added by @charliermarsh on 2024-05-10 01:18_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-05-10 01:37_

---

_Review requested from @dhruvmanila by @charliermarsh on 2024-05-10 01:37_

---

_Comment by @github-actions[bot] on 2024-05-10 01:56_

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

_@dhruvmanila approved on 2024-05-10 05:21_

---

_@AlexWaygood approved on 2024-05-10 06:19_

---

_@MichaReiser approved on 2024-05-10 06:44_

This is a high confidence PR :sweat_smile: 

> I believe the value

I quickly checked the lexer. 

* `-` -> becomes a `TokenKind::Minus`
* `lex_number`: Only numbers

So yes, you're correct.

---

_@MichaReiser reviewed on 2024-05-10 06:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/int.rs`:202 on 2024-05-10 06:45_

Any specific reason why you didn't keep the `u8`, `u16` and `u32` implementations? They would have helped to reduce the size of this PR.

---

_@MichaReiser reviewed on 2024-05-10 06:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__lexer__tests__numbers.snap`:82 on 2024-05-10 06:45_

We should update the test. I think this test intentionally captures the case where the `Number` is `Big`.

---

_@charliermarsh reviewed on 2024-05-10 13:20_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/int.rs`:202 on 2024-05-10 13:20_

I think I did, unless I'm mistaken? I removed the `i8`, `i16`, and `i32` implementations, since it's not safe to cast from a signed to an unsigned integer.

---

_@charliermarsh reviewed on 2024-05-10 13:20_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/int.rs`:202 on 2024-05-10 13:20_

(And it shouldn't be possible in the API.)

---

_@MichaReiser reviewed on 2024-05-10 13:29_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/int.rs`:202 on 2024-05-10 13:29_

Removing the `i8`, `i16` etc implementations makes sense. My question is more if we should have changed them to `u8`, `u16` etc instead. 

---

_Merged by @charliermarsh on 2024-05-10 13:35_

---

_Closed by @charliermarsh on 2024-05-10 13:35_

---

_Branch deleted on 2024-05-10 13:35_

---

_@charliermarsh reviewed on 2024-05-10 13:36_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/int.rs`:202 on 2024-05-10 13:36_

Those already exist above, right?

---
