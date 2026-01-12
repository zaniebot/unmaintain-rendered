```yaml
number: 15215
title: "[`pyflakes`] Ignore errors in `@no_type_check` string annotations (`F722`, `F821`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
assignees: []
merged: true
base: main
head: F722
created_at: 2025-01-02T00:12:13Z
updated_at: 2025-01-04T09:54:58Z
url: https://github.com/astral-sh/ruff/pull/15215
synced_at: 2026-01-12T15:55:50Z
```

# [`pyflakes`] Ignore errors in `@no_type_check` string annotations (`F722`, `F821`)

---

_@InSyncWithFoo_

## Summary

Resolves #13824, resolves #14698. Part of this PR was taken from #14615.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-02 00:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-01-02 09:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:2367 on 2025-01-02 09:02_

Should we instead add a `push_type_diagnostic` method to `Checker` so that we don't need to repeat the `!self.semantic.in_no_type_check` check. This would also be closer to how this works in Red Knot.

---

_Label `rule` added by @MichaReiser on 2025-01-02 09:03_

---

_@MichaReiser reviewed on 2025-01-02 09:04_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:733 on 2025-01-02 09:04_

I'd flip the order here so that Ruff's and Red Knot's behavior is identical. 


```suggestion
                    self.visit_decorator(decorator);
                    if self
                        .semantic
                        .match_typing_expr(&decorator.expression, "no_type_check")
                    {
                        self.semantic.flags |= SemanticModelFlags::NO_TYPE_CHECK;
                    }
```

---

_@InSyncWithFoo reviewed on 2025-01-02 09:20_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/checkers/ast/mod.rs`:2367 on 2025-01-02 09:20_

I'm not sure. I think having such a method is a bit misleading when we don't support `# type: ignore`.

---

_@MichaReiser reviewed on 2025-01-02 11:19_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:2367 on 2025-01-02 11:19_

That's fair. But having the method would prepare us to add `type: ignore` support because it would provide a centralized place to where such a check could be added (vs changing all call sites)

---

_Merged by @MichaReiser on 2025-01-03 09:05_

---

_Closed by @MichaReiser on 2025-01-03 09:05_

---

_Branch deleted on 2025-01-04 09:54_

---
