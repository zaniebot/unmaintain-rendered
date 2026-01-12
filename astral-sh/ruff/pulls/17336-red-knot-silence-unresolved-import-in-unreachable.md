```yaml
number: 17336
title: "[red-knot] Silence `unresolved-import` in unreachable code"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/unreachable-imports
created_at: 2025-04-10T15:44:06Z
updated_at: 2025-04-10T19:13:31Z
url: https://github.com/astral-sh/ruff/pull/17336
synced_at: 2026-01-12T15:56:01Z
```

# [red-knot] Silence `unresolved-import` in unreachable code

---

_@sharkdp_

## Summary

Similar to what we did for `unresolved-reference` and `unresolved-attribute`, we now also silence `unresolved-import` diagnostics if the corresponding `import` statement is unreachable.

This addresses the (already closed) issue #17049.

## Test Plan

Adapted Markdown tests.

---

_Label `red-knot` added by @sharkdp on 2025-04-10 15:44_

---

_Renamed from "[red-knot] Silence unresolved-import in unreachable code" to "[red-knot] Silence `unresolved-import` in unreachable code" by @sharkdp on 2025-04-10 15:45_

---

_Comment by @github-actions[bot] on 2025-04-10 15:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
isort (https://github.com/pycqa/isort)
- error[lint:unresolved-import] /tmp/mypy_primer/projects/isort/isort/settings.py:52:16: Cannot resolve import `tomllib`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/isort/isort/settings.py:54:15: Cannot resolve import `._vendored`
- Found 59 diagnostics
+ Found 57 diagnostics

async-utils (https://github.com/mikeshardmind/async-utils)
- error[lint:unresolved-import] /tmp/mypy_primer/projects/async-utils/_misc/_ensure_annotations.py:57:16: Cannot resolve import `annotationlib`
- Found 31 diagnostics
+ Found 30 diagnostics

black (https://github.com/psf/black)
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/handle_ipynb_magics.py:14:24: Module `typing` has no member `TypeGuard`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/cache.py:20:24: Module `typing` has no member `Self`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/files.py:18:16: Cannot resolve import `tomllib`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/nodes.py:10:24: Module `typing` has no member `TypeGuard`
- Found 258 diagnostics
+ Found 254 diagnostics

rich (https://github.com/Textualize/rich)
- error[lint:unresolved-import] /tmp/mypy_primer/projects/rich/tests/test_syntax.py:26:10: Cannot resolve import `importlib_metadata`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/rich/docs/source/conf.py:27:10: Cannot resolve import `importlib_metadata`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/rich/rich/progress.py:43:24: Module `typing` has no member `Self`
- Found 794 diagnostics
+ Found 791 diagnostics

```
</details>


---

_Marked ready for review by @sharkdp on 2025-04-10 15:49_

---

_Review requested from @carljm by @sharkdp on 2025-04-10 15:49_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-10 15:49_

---

_Review requested from @dcreager by @sharkdp on 2025-04-10 15:49_

---

_@sharkdp reviewed on 2025-04-10 15:50_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3300 on 2025-04-10 15:50_

We could also move the `is_import_reachable` check inside `report_unresolved_module`, but that would require passing many parameters (or the closure). Not sure what's better.

---

_@AlexWaygood reviewed on 2025-04-10 16:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3300 on 2025-04-10 16:03_

Or we could get rid of the `report_unresolved_module` function and replace it with a method on the `TypeInferenceBuilder`? Something like this?

```rs
impl TypeInferenceBuilder {
    fn report_unresolved_module<'a>(
        &self,
        node: impl Into<AnyNodeRef<'a>>,
        range: impl Ranged,
        level: u32,
        module: Option<&str>
    ) {
        let file_scope_id = self.scope().file_scope_id(self.db());

        if !self.index
            .is_node_reachable(self.db(), file_scope_id, NodeKey::from_node(node))
        {
            return;
        }

        self.context.report_lint(
            &UNRESOLVED_IMPORT,
            range,
            format_args!(
                "Cannot resolve import `{}{}`",
                ".".repeat(level as usize),
                module.unwrap_or_default()
            ),
        );
    }
}

---

_@carljm approved on 2025-04-10 16:05_

---

_@AlexWaygood reviewed on 2025-04-10 16:11_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3300 on 2025-04-10 16:11_

(could also just have the method accept `NodeKey` and `TextRange` to avoid monomorphisation)

---

_@AlexWaygood approved on 2025-04-10 16:11_

---

_@sharkdp reviewed on 2025-04-10 19:04_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3300 on 2025-04-10 19:04_

Thanks!

---

_Merged by @sharkdp on 2025-04-10 19:13_

---

_Closed by @sharkdp on 2025-04-10 19:13_

---

_Branch deleted on 2025-04-10 19:13_

---
