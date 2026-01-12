```yaml
number: 1103
title: "[internal] mdtest silently ignores \"python\" code blocks"
type: issue
state: closed
author: JelleZijlstra
labels:
  - bug
  - testing
assignees: []
created_at: 2025-08-28T15:56:09Z
updated_at: 2025-08-28T16:59:04Z
url: https://github.com/astral-sh/ty/issues/1103
synced_at: 2026-01-12T15:54:24Z
```

# [internal] mdtest silently ignores "python" code blocks

---

_@JelleZijlstra_

If you use a code block tagged with "python" instead of "py" in an mdtest, it's silently ignored. For example, after this patch the relevant test still passes:

````diff
diff --git a/crates/ty_python_semantic/resources/mdtest/import/case_sensitive.md b/crates/ty_python_semantic/resources/mdtest/import/case_sensitive.md
index f822ba47e2..8956e4dac1 100644
--- a/crates/ty_python_semantic/resources/mdtest/import/case_sensitive.md
+++ b/crates/ty_python_semantic/resources/mdtest/import/case_sensitive.md
@@ -22,7 +22,7 @@ class Foo:
 ```python
 from a import Foo
 
-reveal_type(Foo().x)  # revealed: int
+reveal_type(Foo().x)  # revealed: int42
 ```
 
````


(Noticed this while working on something else; might fix it later but no guarantees.)

---

_Label `bug` added by @AlexWaygood on 2025-08-28 16:04_

---

_Label `testing` added by @AlexWaygood on 2025-08-28 16:04_

---

_Closed by @carljm on 2025-08-28 16:59_

---
