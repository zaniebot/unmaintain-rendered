```yaml
number: 20993
title: "[ty] Fix completions at end of file"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/fix-completion-at-end-of-file
created_at: 2025-10-20T12:25:47Z
updated_at: 2025-10-21T09:36:29Z
url: https://github.com/astral-sh/ruff/pull/20993
synced_at: 2026-01-12T15:57:14Z
```

# [ty] Fix completions at end of file

---

_@MichaReiser_

## Summary
Fixes https://github.com/astral-sh/ty/issues/1392

The methods on `Tokens` incorrectly assumed that there can be at most one token starting or ending at `offset`. 
Unfortunately, this isn't the case because Ruff's lexer can produce zero-length tokens. The most common
zero length tokens are the `Newline` at the end of the file (The lexer must emit this token because every logical line must end with a newline token), 
or `Dedent` tokens (which are emitted when there are "fewer" indents, they're always zero length).

The fix in this PR is to make the methods on `Tokens` aware of this and return the first token starting at 
or last token ending at a given offset (instead of any token starting or ending at).

## Test Plan

Added test


---

_Label `bug` added by @MichaReiser on 2025-10-20 12:25_

---

_Label `server` added by @MichaReiser on 2025-10-20 12:25_

---

_Label `ty` added by @MichaReiser on 2025-10-20 12:25_

---

_Review requested from @carljm by @MichaReiser on 2025-10-20 12:25_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-10-20 12:25_

---

_Review requested from @sharkdp by @MichaReiser on 2025-10-20 12:25_

---

_Review requested from @dcreager by @MichaReiser on 2025-10-20 12:25_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-10-20 12:25_

---

_@sharkdp reviewed on 2025-10-20 12:38_

---

_Review comment by @sharkdp on `crates/ty_ide/src/completion.rs`:1467 on 2025-10-20 12:38_

I also had a regression test here: https://github.com/astral-sh/ruff/pull/20951/files. That `Test` protocol seems overly complicated here?

---

_Comment by @github-actions[bot] on 2025-10-20 23:28_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-20 23:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-20 23:36_

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

_Review comment by @sharkdp on `crates/ruff_python_parser/src/lib.rs`:519 on 2025-10-21 06:45_

```suggestion
    /// it returns the index of the last one. Multiple tokens can end at the same offset in cases where
```

---

_Review comment by @sharkdp on `crates/ruff_python_parser/src/lib.rs`:510 on 2025-10-21 07:34_

An alternative functional form would be the following. I'll leave it up to you which one you prefer.
```suggestion
            Ok(idx) => {
                // Some tokens have an empty range (e.g. `Newline` at the end of the file or `Dedent`).
                // For those, we want to get the first token starting at the given offset.
                Ok((0..=idx)
                    .rev()
                    .take_while(|&i| self[i].start() == offset)
                    .last()
                    .unwrap_or(idx))
```

---

_Review comment by @sharkdp on `crates/ruff_python_parser/src/lib.rs`:539 on 2025-10-21 07:35_

Similar here
```suggestion
            Ok(idx) => {
                // Some tokens have an empty range (e.g. `Newline` at the end of the file or `Dedent`).
                // For those, we want to return the last token that ends at the given offset. That's why
                // we need to walk backwards.
                Ok((idx..me.len())
                    .take_while(|&i| me[i].end() == offset)
                    .last()
                    .unwrap_or(idx))
```

---

_@sharkdp approved on 2025-10-21 07:37_

Thank you!

Initially, I was hoping that something like
```rs
self.binary_search_by_key(&(offset, 0), |token: &Token| {
    (token.start(), std::ptr::from_ref(token) as usize)
})
```
would also work. It does always find the first element, but it obviously changes all `Ok(…)`s into `Err(…)`s, and some callers rely on that distinction.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:510 on 2025-10-21 07:42_

I think I could maybe use `partition` instead

---

_@MichaReiser reviewed on 2025-10-21 07:42_

---

_Review comment by @sharkdp on `crates/ruff_python_parser/src/lib.rs`:510 on 2025-10-21 07:47_

`slice::partition_point`? Yes, that could work, but you would need to do an additional comparison to decide whether it's an `Ok` or `Err` variant.

---

_@sharkdp reviewed on 2025-10-21 07:47_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-10-21 08:05_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:649 on 2025-10-21 09:03_

nit: `tokens` instead of `me`?

---

_@dhruvmanila approved on 2025-10-21 09:05_

---

_Merged by @MichaReiser on 2025-10-21 09:24_

---

_Closed by @MichaReiser on 2025-10-21 09:24_

---

_Branch deleted on 2025-10-21 09:24_

---
