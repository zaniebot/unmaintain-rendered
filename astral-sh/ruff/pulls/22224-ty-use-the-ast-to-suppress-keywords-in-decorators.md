```yaml
number: 22224
title: "[ty] Use the AST to suppress keywords in decorators"
type: pull_request
state: merged
author: RasmusNygren
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: fix-decorator-completions
created_at: 2025-12-27T13:10:09Z
updated_at: 2025-12-27T14:14:29Z
url: https://github.com/astral-sh/ruff/pull/22224
synced_at: 2026-01-12T15:57:44Z
```

# [ty] Use the AST to suppress keywords in decorators

---

_@RasmusNygren_

As of 014abe1ee19e617c13ac498d09de5ae2f932f6e8 the detection of decorator nodes in the AST is better and we no longer need to fall back to token-matching to detect decorators. This makes the logic significantly simpler and fixes some bugs from inaccuracies in the state machine.

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
Fixes https://github.com/astral-sh/ruff/pull/22177#issuecomment-3690416929

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
New tests for known bugs in previous implementation and fix related TODO tests


---

_Review requested from @carljm by @RasmusNygren on 2025-12-27 13:10_

---

_Review requested from @MichaReiser by @RasmusNygren on 2025-12-27 13:10_

---

_Review requested from @AlexWaygood by @RasmusNygren on 2025-12-27 13:10_

---

_Review requested from @sharkdp by @RasmusNygren on 2025-12-27 13:10_

---

_Review requested from @dcreager by @RasmusNygren on 2025-12-27 13:10_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:781 on 2025-12-27 13:12_

We should stop when finding the first statement to avoid traversing the entire ancestor chain.

---

_@MichaReiser approved on 2025-12-27 13:12_

Thank you :)

---

_@RasmusNygren reviewed on 2025-12-27 13:25_

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:781 on 2025-12-27 13:25_

Very good point, fixed! :)

---

_Label `server` added by @MichaReiser on 2025-12-27 13:28_

---

_Label `ty` added by @MichaReiser on 2025-12-27 13:28_

---

_Merged by @MichaReiser on 2025-12-27 13:29_

---

_Closed by @MichaReiser on 2025-12-27 13:29_

---

_Branch deleted on 2025-12-27 14:14_

---
