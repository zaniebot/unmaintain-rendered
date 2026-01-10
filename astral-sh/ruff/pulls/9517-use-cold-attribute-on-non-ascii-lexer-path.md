```yaml
number: 9517
title: "Use `#[cold]` attribute on non-ASCII lexer path"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/cold
created_at: 2024-01-15T01:45:04Z
updated_at: 2024-01-18T03:16:47Z
url: https://github.com/astral-sh/ruff/pull/9517
synced_at: 2026-01-10T22:57:09Z
```

# Use `#[cold]` attribute on non-ASCII lexer path

---

_Pull request opened by @charliermarsh on 2024-01-15 01:45_

Just curious if this shows up.

---

_Renamed from "Use #[cold] attribute on non-ASCII lexer path" to "Use `#[cold]` attribute on non-ASCII lexer path" by @charliermarsh on 2024-01-15 01:45_

---

_Comment by @github-actions[bot] on 2024-01-15 02:03_

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

_Comment by @charliermarsh on 2024-01-15 02:03_

Seems like a 1% improvement on all lexer benchmarks (but doesn't meet the CodSpeed threshold to post): https://codspeed.io/astral-sh/ruff/branches/charlie/cold

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/src/lexer.rs`:1492 on 2024-01-15 14:43_

Can you try adding `#[inline(never)]` and to `is_unicode_identifier_continue` as well?

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/src/lexer.rs`:1516 on 2024-01-15 14:46_

How come you're forcing 64-bit alignment here?

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/src/lexer.rs`:1503 on 2024-01-15 14:48_

If you're looking to speed things up here for the ASCII fast path, it might make sense to refactor the calling code to deal with this `&str` chunks instead. That way, you can get entire identifiers all in one go if that's the common path. (But if it's not the overwhelmingly common path, and it might not be, then it may not be such a good idea.) This _could_ also be a good "started SIMD" task.

---

_@BurntSushi reviewed on 2024-01-15 14:49_

---

_@charliermarsh reviewed on 2024-01-15 14:52_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/lexer.rs`:1503 on 2024-01-15 14:52_

This sounds interesting even just to learn. Can you say a bit more here? What's the chunk?

---

_@charliermarsh reviewed on 2024-01-15 14:52_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/lexer.rs`:1516 on 2024-01-15 14:52_

(This is copied out from the `unicode_ident` crate just for quickly benchmarking.)

---

_@BurntSushi reviewed on 2024-01-15 15:05_

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/src/lexer.rs`:1503 on 2024-01-15 15:05_

To be clear, I don't have any understanding of what the caller code looks like. So the idea may be bunk. But basically, if you're calling this function over and over on each individual codepoint, then you're paying a price for the latency of doing that check. If it's a _necessary_ payment, then there is not much to do. But if "a starting identifier char followed by continuation chars" is extremely common, then it may make more sense to have an API like this:

```rust
fn get_ident_from_prefix(src: &str) -> Option<Identifier> { ... }
```

The idea is that if you're _pretty_ sure there's an identifier there in most cases, then you make the uncommon cases a little slower in favor of making the common case a lot faster. The idea here is that you might be able to detect identifiers very quickly with bit tricks and/or SIMD. The specific details of what the bit tricks/SIMD are is left as an exercise to for the reader. :)

---

_@MichaReiser reviewed on 2024-01-15 15:30_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1503 on 2024-01-15 15:30_

The calling code is very simple. We call it for every character until the end of an identifier.



```
        self.cursor.eat_while(is_identifier_continuation);

```


It eats the characters why `is_identifier_continuation` is true, and we expect most characters to be ascii. There are probably some SIMD tricks that we could apply but it never felt worthwhile considering that lexing used to be a very small fraction compared to parsing.


It could make sense to add an explicit branch for all python whitespace to avoid calling into the function when we reach any space character (the end of an identifier).

---

_@BurntSushi reviewed on 2024-01-15 15:33_

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/src/lexer.rs`:1503 on 2024-01-15 15:33_

Oh yeah absolutely, if lexing is already a small slice of the pie, it may indeed not make sense to expend a lot of effort here.

---

_@MichaReiser reviewed on 2024-01-15 15:36_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1503 on 2024-01-15 15:36_

It might be worth it with the new parser ;)

---

_Closed by @charliermarsh on 2024-01-18 03:16_

---
