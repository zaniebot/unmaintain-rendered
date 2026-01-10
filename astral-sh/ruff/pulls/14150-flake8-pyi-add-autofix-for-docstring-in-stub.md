```yaml
number: 14150
title: "[`flake8-pyi`] Add autofix for `docstring-in-stub` (`PYI021`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - fixes
assignees: []
merged: true
base: main
head: PYI021
created_at: 2024-11-07T11:00:18Z
updated_at: 2024-11-07T20:18:02Z
url: https://github.com/astral-sh/ruff/pull/14150
synced_at: 2026-01-10T20:50:57Z
```

# [`flake8-pyi`] Add autofix for `docstring-in-stub` (`PYI021`)

---

_Pull request opened by @InSyncWithFoo on 2024-11-07 11:00_

## Summary

Resolves #14123.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-11-07 11:00_

---

_Comment by @InSyncWithFoo on 2024-11-07 11:05_

The fix does not always result in optimal output, but IIRC these kinds of problems are for the formatter to solve.

```diff
 class Foo:
-    """Foo"""  # qux
+      # qux
```



---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/docstring_in_stubs.rs`:62 on 2024-11-07 11:07_

In a stub file this fix will cause us to start emitting https://docs.astral.sh/ruff/rules/pass-statement-stub-body/ on the stub. You should replace the docstring with `...` instead of `pass`.

---

_@AlexWaygood requested changes on 2024-11-07 11:08_

Thanks!

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-11-07 11:12_

---

_@MichaReiser reviewed on 2024-11-07 11:17_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/docstring_in_stubs.rs`:64 on 2024-11-07 11:17_

Nit
```suggestion
        Edit::range_deletion(range)
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI021_PYI021.pyi.snap`:56 on 2024-11-07 11:18_

I think the tests still need updating
```suggestion
  7 |+    ...  # ERROR PYI021
```

---

_@MichaReiser reviewed on 2024-11-07 11:18_

---

_Comment by @github-actions[bot] on 2024-11-07 11:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-11-07 11:55_

Thanks!

---

_Renamed from "[`flake8-pyi`] Remove docstring fix (`PYI021`)" to "[`flake8-pyi`] Add autofix for `docstring-in-stub` (`PYI021`)" by @AlexWaygood on 2024-11-07 11:57_

---

_Label `rule` added by @AlexWaygood on 2024-11-07 11:57_

---

_Label `fixes` added by @AlexWaygood on 2024-11-07 11:57_

---

_Merged by @AlexWaygood on 2024-11-07 12:00_

---

_Closed by @AlexWaygood on 2024-11-07 12:00_

---

_Branch deleted on 2024-11-07 20:18_

---
