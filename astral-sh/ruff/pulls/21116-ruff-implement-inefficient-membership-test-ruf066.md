```yaml
number: 21116
title: "[`ruff`] Implement `inefficient-membership-test` (`RUF066`)"
type: pull_request
state: closed
author: Zaczero
labels:
  - rule
  - needs-decision
assignees: []
base: main
head: ruff-066-inefficient-membership-test
created_at: 2025-10-28T22:43:09Z
updated_at: 2025-10-29T19:09:49Z
url: https://github.com/astral-sh/ruff/pull/21116
synced_at: 2026-01-12T15:57:16Z
```

# [`ruff`] Implement `inefficient-membership-test` (`RUF066`)

---

_@Zaczero_

Closes https://github.com/astral-sh/ruff/issues/19933

## Summary

Add new rule to detect membership tests against list, tuple, or set literals containing complex elements that prevent Python's `LOAD_CONST` bytecode optimization.

When containers have complex elements (nested lists, dicts, function calls, operations), Python reconstructs the container on every membership test, causing performance degradation in hot code paths.

The rule detects:
- Lists/tuples with complex elements (nested containers, dicts, function calls, operations, lambdas)
- Sets with complex elements (function calls, operations, etc.)
- Both `in` and `not in` operators

Provides unsafe autofix converting to equality comparisons:
- `x in [a, b]` → `x == a or x == b`
- `x not in [a, b]` → `x != a and x != b`

Fix is marked unsafe because it changes evaluation semantics (short-circuit behavior and potential side effects in custom `__eq__`).

## Test Plan

Implementing RUFF066.py test case.

I verified locally whether this performance optimization applies to non-trivial lists and sets of any size, and it appears it does. There's always a performance win, but the smaller the collection, the better.

---

_Comment by @Zaczero on 2025-10-28 22:45_

I wanted to push forward the idea of https://github.com/astral-sh/ruff/issues/19933 and arrogantly implemented a functional rule. I am really new to ruff and have fairly basic Rust knowledge so it may need improvement.

cc @ntBre since you interacted

---

_Label `rule` added by @amyreese on 2025-10-29 00:05_

---

_Label `needs-decision` added by @amyreese on 2025-10-29 00:05_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/ruff/rules/inefficient_membership_test.rs`:53 on 2025-10-29 13:45_

More reasons:
* `in` also checks for identity: compare `math.nan in [math.nan]` to `math.nan == math.nan`.
* The left argument of `in` may have a side effect.
* `in` always returns a `bool` but `==` can return anything.

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/ruff/rules/inefficient_membership_test.rs`:138 on 2025-10-29 13:45_

Not all operations on literals are const-folded, e.g. `"%s" % 3`.

---

_@dscorbett reviewed on 2025-10-29 13:47_

---

_Comment by @ntBre on 2025-10-29 13:54_

Thanks for your work on this! I quickly skimmed the implementation, and it looks good overall :)

However, we're not really prioritizing adding new rules at the moment, so I'd like to hold off a bit longer to see if there's more interest in this rule. I'll approve the workflow run so that we can get an ecosystem report, though.

---

_@Zaczero reviewed on 2025-10-29 14:08_

---

_Review comment by @Zaczero on `crates/ruff_linter/src/rules/ruff/rules/inefficient_membership_test.rs`:138 on 2025-10-29 14:08_

TYVM for very nice findings! I'll resolve it when and if ruff decides to include such rule.

---

_Comment by @MichaReiser on 2025-10-29 15:45_

We've been hesitant about adding rules that encode specific optimizations that Python does because that can change between Python versions and is out of our control or might even become irrelevant. That's why this rule will require some more discussion before being added and I'll close this PR for now, as there's no clear path forward. 

But thanks for your work. It's a great starting point if we decide to implement this rule.

---

_Closed by @MichaReiser on 2025-10-29 15:45_

---

_Comment by @Zaczero on 2025-10-29 17:01_

No prob, I was surprised by how convenient it was to add a new rule and test it in Ruff. Good job on that!

---

_Comment by @MichaReiser on 2025-10-29 19:09_

Thanks for the kind words and for being so understanding. 

---
