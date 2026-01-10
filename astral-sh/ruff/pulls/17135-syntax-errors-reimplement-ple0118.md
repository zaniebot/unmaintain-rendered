```yaml
number: 17135
title: "[syntax-errors] Reimplement PLE0118"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/syn-ple0118
created_at: 2025-04-01T21:31:26Z
updated_at: 2025-04-02T13:09:50Z
url: https://github.com/astral-sh/ruff/pull/17135
synced_at: 2026-01-10T19:40:37Z
```

# [syntax-errors] Reimplement PLE0118

---

_Pull request opened by @ntBre on 2025-04-01 21:31_

Summary
--

This PR reimplements
[load-before-global-declaration (PLE0118)](https://docs.astral.sh/ruff/rules/load-before-global-declaration/) as a semantic syntax error.

I added a `global` method to the `SemanticSyntaxContext` trait to make this very easy, at least in ruff. Does red-knot have something similar?

If this approach will also work in red-knot, I think some of the other PLE rules are also compile-time errors in CPython, PLE0117 in particular. 0115 and 0116 also mention `SyntaxError`s in their docs, but I haven't confirmed them in the REPL yet.

Test Plan
--

Existing linter tests for PLE0118. I think this actually can't be tested very easily in an inline test because the `TestContext` doesn't have a real way to track globals.


---

_Review requested from @MichaReiser by @ntBre on 2025-04-01 21:31_

---

_Review requested from @dhruvmanila by @ntBre on 2025-04-01 21:31_

---

_Label `internal` added by @ntBre on 2025-04-01 21:31_

---

_Comment by @github-actions[bot] on 2025-04-01 21:41_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:739 on 2025-04-02 07:46_

```suggestion
    /// Return the [`TextRange`] at which a name is declared as `global` in the current scope.
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:793 on 2025-04-02 07:47_

Red Knot doesn't support globals yet, so it's hard to say if this will be supported in Red Knot and how ;)

It does have the downside that this check will not work in the test context. And it also means that it requires one extra traversal of the body to get the globals first but I don't see how we can avoid that.

But we (you) can worry about that later ;)


---

_@MichaReiser approved on 2025-04-02 07:50_

---

_@ntBre reviewed on 2025-04-02 12:57_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:793 on 2025-04-02 12:57_

Haha sounds good :)

---

_Merged by @ntBre on 2025-04-02 13:03_

---

_Closed by @ntBre on 2025-04-02 13:03_

---

_Branch deleted on 2025-04-02 13:03_

---
