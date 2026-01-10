```yaml
number: 16097
title: "[`flake8-builtins`] Update documentation (`A005`)"
type: pull_request
state: merged
author: ntBre
labels:
  - documentation
assignees: []
merged: true
base: main
head: brent/a005-docs
created_at: 2025-02-11T13:53:08Z
updated_at: 2025-02-12T17:50:16Z
url: https://github.com/astral-sh/ruff/pull/16097
synced_at: 2026-01-10T19:57:22Z
```

# [`flake8-builtins`] Update documentation (`A005`)

---

_Pull request opened by @ntBre on 2025-02-11 13:53_

Follow-up to https://github.com/astral-sh/ruff/pull/15951 to update
* the options links in A005 to reference `lint.flake8-builtins.builtins-strict-checking`
* the description of the rule to explain strict vs non-strict checking
* the option documentation to point back to the rule

---

_Label `documentation` added by @ntBre on 2025-02-11 13:53_

---

_Comment by @github-actions[bot] on 2025-02-11 13:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @MichaReiser by @ntBre on 2025-02-11 14:00_

---

_@DaniBodor reviewed on 2025-02-11 14:37_

---

_Review comment by @DaniBodor on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:30 on 2025-02-11 14:37_

perhaps re-use example from above `a/logging.py` (or use the `utils/logging.py` example above)?

---

_@ntBre reviewed on 2025-02-11 16:48_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:30 on 2025-02-11 16:48_

I think that's a good idea, thanks! I went for a consistent use of the `logging` example:

https://github.com/astral-sh/ruff/blob/6e64c4c93d6b89426e7e8c487055d7061504efc2/crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs#L26-L30

What do you think?

---

_@MichaReiser reviewed on 2025-02-11 19:01_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:31 on 2025-02-11 19:01_

We should document here or on the option the difference between preview and non preview. The way I remember it is that the default changes based on preview mode.

---

_@MichaReiser approved on 2025-02-12 17:45_

---

_Merged by @ntBre on 2025-02-12 17:50_

---

_Closed by @ntBre on 2025-02-12 17:50_

---

_Branch deleted on 2025-02-12 17:50_

---
