```yaml
number: 19570
title: "[ty] Add stub mapping support to signature help"
type: pull_request
state: merged
author: UnboundVariable
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: sig_help
created_at: 2025-07-26T18:02:00Z
updated_at: 2025-07-28T19:43:47Z
url: https://github.com/astral-sh/ruff/pull/19570
synced_at: 2026-01-12T15:56:42Z
```

# [ty] Add stub mapping support to signature help

---

_@UnboundVariable_

This PR improves the "signature help" language server feature in two ways:
1. It adds support for the recently-introduced "stub mapper" which maps symbol declarations within stubs to their implementation counterparts. This allows the signature help to display docstrings from the original implementation.
2. It incorporates a more robust fix to a bug that was addressed in a [previous PR](https://github.com/astral-sh/ruff/pull/19542). It also adds more comprehensive tests to cover this case.



---

_Review requested from @carljm by @UnboundVariable on 2025-07-26 18:02_

---

_Review requested from @MichaReiser by @UnboundVariable on 2025-07-26 18:02_

---

_Review requested from @AlexWaygood by @UnboundVariable on 2025-07-26 18:02_

---

_Review requested from @sharkdp by @UnboundVariable on 2025-07-26 18:02_

---

_Review requested from @dcreager by @UnboundVariable on 2025-07-26 18:02_

---

_Comment by @github-actions[bot] on 2025-07-26 18:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `server` added by @AlexWaygood on 2025-07-26 18:27_

---

_Label `ty` added by @AlexWaygood on 2025-07-26 18:27_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/signature_help.rs`:103 on 2025-07-28 08:26_

What's the benefit of using a 1 byte offset? I think this is a bit dangerous as it might panic if anyone ends up using the passed range to take a peek at the source text (or calls any method on `tokens` where some methods enforce that the range falls directly at a token boundary). 

It might be better to use the actual len of the character at `offset` over using an incorrect range.

---

_@MichaReiser approved on 2025-07-28 08:26_

---

_@dhruvmanila approved on 2025-07-28 09:17_

Thanks!

---

_@UnboundVariable reviewed on 2025-07-28 15:46_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/signature_help.rs`:103 on 2025-07-28 15:46_

This is currently required to "trick" `covering_node` to not return the node to the left of the cursor position. Is there a reason why `covering_node` returns a node to the left of a zero-length range? Do other callers rely on this behavior? If not, perhaps the best fix here is to modify `covering_node` to not return the node to the left of a zero-length range?

Is there an existing function that retrieves the length of the character at `offset`?

---

_Merged by @UnboundVariable on 2025-07-28 17:57_

---

_Closed by @UnboundVariable on 2025-07-28 17:57_

---

_Branch deleted on 2025-07-28 17:57_

---

_@MichaReiser reviewed on 2025-07-28 19:43_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/signature_help.rs`:103 on 2025-07-28 19:43_

To get the size of a character at a given offset:

```rust
a[offset..].chars().next().map(TextLen::text_len);
```

> Is there a reason why covering_node returns a node to the left of a zero-length range?

`covering_node` uses `contains` to test if two ranges overlap. Which I find the correct behavior in case we start with a range (in which case the nodes don't overlap). 

The idea behind the API is that `covering_node` is very low level and unopinionated. It's also not the idea that it is used directly with an offset because there are cases where your offset falls directly between two nodes, in which case it requires some prioritization. The way we've implemented this in other LSP methods is by using `tokens.at`


```rust
let token = parsed
        .tokens()
        .at_offset(offset)
        .max_by_key(|token| match token.kind() {
            TokenKind::Name
            | TokenKind::String
            | TokenKind::Complex
            | TokenKind::Float
            | TokenKind::Int => 1,
            _ => 0,
        })?;
```

This allows the caller to customize which tokens should be preferred. I'm not sure what the heuristic should be for signature help. We can consider extracting the above logic from `find_goto_target` if it is the same or we can deploy our own here.

---
