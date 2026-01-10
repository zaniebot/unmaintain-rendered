```yaml
number: 19542
title: "[ty] Fix bug #879 in signature help"
type: pull_request
state: merged
author: UnboundVariable
labels:
  - ty
assignees: []
merged: true
base: main
head: sig_help_bug
created_at: 2025-07-24T23:15:00Z
updated_at: 2025-07-29T20:14:49Z
url: https://github.com/astral-sh/ruff/pull/19542
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Fix bug #879 in signature help

---

_Pull request opened by @UnboundVariable on 2025-07-24 23:15_

This PR fixes bug [#879](https://github.com/astral-sh/ty/issues/879) where the signature help popup remains visible after typing the closing paren in a call expression.

---

_Review requested from @carljm by @UnboundVariable on 2025-07-24 23:15_

---

_Review requested from @MichaReiser by @UnboundVariable on 2025-07-24 23:15_

---

_Review requested from @AlexWaygood by @UnboundVariable on 2025-07-24 23:15_

---

_Review requested from @sharkdp by @UnboundVariable on 2025-07-24 23:15_

---

_Review requested from @dcreager by @UnboundVariable on 2025-07-24 23:15_

---

_Review requested from @dhruvmanila by @UnboundVariable on 2025-07-24 23:15_

---

_Comment by @github-actions[bot] on 2025-07-24 23:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @UnboundVariable on 2025-07-24 23:26_

---

_Closed by @UnboundVariable on 2025-07-24 23:26_

---

_Branch deleted on 2025-07-24 23:26_

---

_Review comment by @carljm on `crates/ty_ide/src/signature_help.rs`:71 on 2025-07-24 23:26_

Given the doc comment on `get_call_expr`, it seems like a violation of its contract (aka a bug) for it to return a call expression that ends before `offset`. So wouldn't it be slighly better to handle this inside `get_call_expr`?

---

_@carljm approved on 2025-07-24 23:27_

Thank you!

---

_Comment by @dhruvmanila on 2025-07-25 04:21_

I don't think this is an issue but just wanted to point it out.

Wouldn't this lead to signature help disappearing for nested calls? For example, in the `foo` call in the video, once I type the `foo(bar()<CURSOR>)` the signature help disappears and would appear only with the `,` (trigger character). But, the behavior is different for the `baz` call where the window stays.

https://github.com/user-attachments/assets/a85e6299-78bb-482f-8159-1653e566eb41



---

_@UnboundVariable reviewed on 2025-07-26 18:04_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/signature_help.rs`:71 on 2025-07-26 18:04_

Yes, you're correct. This fix wasn't sound. I've introduced a better fix — along with some tests to cover this case — in a [new PR](https://github.com/astral-sh/ruff/pull/19570). The problem is that the old code was using a zero-length range when calling `covering_node`, and this returned the node to the left of the cursor position. 

---

_Comment by @UnboundVariable on 2025-07-26 18:04_

@dhruvmanila, yes, you're correct. This fix wasn't sound. I've introduced a better fix — along with some tests to cover this case — in a [new PR](https://github.com/astral-sh/ruff/pull/19570).

---

_Label `ty` added by @ntBre on 2025-07-29 20:14_

---
