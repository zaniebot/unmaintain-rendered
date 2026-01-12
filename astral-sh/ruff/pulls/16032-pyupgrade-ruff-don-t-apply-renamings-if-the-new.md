```yaml
number: 16032
title: "[`pyupgrade`] [`ruff`] Don't apply renamings if the new name is shadowed in a scope of one of the references to the binding (`UP049`, `RUF052`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: UP049
created_at: 2025-02-08T00:29:29Z
updated_at: 2025-02-08T16:52:05Z
url: https://github.com/astral-sh/ruff/pull/16032
synced_at: 2026-01-12T15:55:53Z
```

# [`pyupgrade`] [`ruff`] Don't apply renamings if the new name is shadowed in a scope of one of the references to the binding (`UP049`, `RUF052`)

---

_@InSyncWithFoo_

## Summary

Helps with #16024.

A fix will no longer be offered if the new name is invalid as an identifier or if it shadows a type parameter from an outer scope, while complex stringified type hints will cause it to be marked as unsafe.

The error message has also been changed from "Remove the leading underscores" to "Rename type parameter to remove leading underscores", as the former would not be the best suggestion when the new name is not a valid identifier (e.g., `_0`).

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-02-08 00:29_

---

_Comment by @github-actions[bot] on 2025-02-08 00:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @InSyncWithFoo on 2025-02-08 00:38_

@ntBre `Renamer` has a small problem in that it would also rename `_T` in this case:

```python
class C[_T]:
	def f[T](self):
		a: _T = ...
```

The new logic introduced in this PR <em>does</em> check for existing bindings for `T`, but only at the top of the class, so it won't be able to detect bindings declared within inner scopes. This is a general problem with `Renamer`, so I decided not to fix it for now; instead, I added [a few test cases](https://github.com/astral-sh/ruff/pull/16032/files#diff-648c2dda3437ddd6741025bf549e4114ebf45d326ad4ec058d74a17fa7e83326R93-R104).

---

_@AlexWaygood reviewed on 2025-02-08 10:33_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:164 on 2025-02-08 10:33_

I don't think checking this again is necessary; no tests fail if I remove this added code. We already check this on line 133 with the `ShadowedKind` logic.

```suggestion
```

---

_@AlexWaygood reviewed on 2025-02-08 10:37_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:146 on 2025-02-08 10:37_

I was discussing this issue with @ntBre on Friday -- we think the issue with replacing "complex" stringized references is probably a result of us setting an incorrect range somewhere for `ResolvedReference`s where the reference is a "complex" stringized reference. I'm okay with this for now, but we should try to see if we can fix the underlying bug rather than working around it repeatedly

---

_Label `bug` added by @AlexWaygood on 2025-02-08 11:06_

---

_Label `fixes` added by @AlexWaygood on 2025-02-08 11:06_

---

_@AlexWaygood approved on 2025-02-08 11:19_

Thanks! I pushed a few changes so that this is fixed more holistically, and added some test cases for RUF052 as well.

---

_Comment by @AlexWaygood on 2025-02-08 11:22_

This doesn't close #16024, however. Even on this branch, we get this behaviour:

```
~/dev/ruff (UP049)⚡ [1] % cargo run -- check --no-cache --preview --diff --select=UP049 --target-version=py312 foo.py
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.12s
     Running `target/debug/ruff check --no-cache --preview --diff --select=UP049 --target-version=py312 foo.py`
--- foo.py
+++ foo.py
@@ -1 +1 @@
-class Foo[_T, __T]: ...
+class Foo[T, T]: ...

Would fix 2 errors.
```

So the fix can still introduce invalid syntax.

---

_Renamed from "[`pyupgrade`] Better fix logic (`UP049`)" to "[`pyupgrade`] [`ruff`] Don't apply renamings if the new name is shadowed in a scope of one of the references to the binding (`UP049`, `RUF052`))" by @AlexWaygood on 2025-02-08 11:23_

---

_Renamed from "[`pyupgrade`] [`ruff`] Don't apply renamings if the new name is shadowed in a scope of one of the references to the binding (`UP049`, `RUF052`))" to "[`pyupgrade`] [`ruff`] Don't apply renamings if the new name is shadowed in a scope of one of the references to the binding (`UP049`, `RUF052`)" by @AlexWaygood on 2025-02-08 11:23_

---

_Merged by @AlexWaygood on 2025-02-08 11:25_

---

_Closed by @AlexWaygood on 2025-02-08 11:25_

---

_@AlexWaygood reviewed on 2025-02-08 16:30_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:146 on 2025-02-08 16:30_

Aha -- it looks like there's already an issue open for the problem about ranges being incorrect for parsed nodes inside complex string annotations. It's here: https://github.com/astral-sh/ruff/issues/10586

---

_Branch deleted on 2025-02-08 16:52_

---
