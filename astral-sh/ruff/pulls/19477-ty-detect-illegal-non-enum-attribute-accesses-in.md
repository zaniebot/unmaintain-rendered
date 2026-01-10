```yaml
number: 19477
title: "[ty] Detect illegal non-enum attribute accesses in Literal annotation"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/illegal-attribute-access-literal
created_at: 2025-07-22T07:21:35Z
updated_at: 2025-07-22T09:42:05Z
url: https://github.com/astral-sh/ruff/pull/19477
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Detect illegal non-enum attribute accesses in Literal annotation

---

_Pull request opened by @sharkdp on 2025-07-22 07:21_

## Summary

Detect illegal attribute accesses in `Literal[X.Y]` annotations if `X` is not an enum class.

## Test Plan

New Markdown test

---

_Review requested from @carljm by @sharkdp on 2025-07-22 07:21_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-22 07:21_

---

_Review requested from @dcreager by @sharkdp on 2025-07-22 07:21_

---

_Label `ty` added by @sharkdp on 2025-07-22 07:21_

---

_Comment by @github-actions[bot] on 2025-07-22 07:25_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-07-22 09:24_

> ```diff
> + error[invalid-type-form] bidict/_typing.py:22:28: Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum value
> ```

I see the error has gone away from the primer diff on the latest version of this PR, but FWIW I think "enum member" would be clearer than "enum value" in this error message. (I know that this issue predates this PR!)


---

_Closed by @sharkdp on 2025-07-22 09:30_

---

_Reopened by @sharkdp on 2025-07-22 09:30_

---

_@AlexWaygood reviewed on 2025-07-22 09:31_

If I apply this diff to your branch then I get some pull-types panics on the `literal.md` mdtest:

````diff
diff --git a/crates/ty_python_semantic/resources/mdtest/annotations/literal.md b/crates/ty_python_semantic/resources/mdtest/annotations/literal.md
index 0c266868e1..6e480724cb 100644
--- a/crates/ty_python_semantic/resources/mdtest/annotations/literal.md
+++ b/crates/ty_python_semantic/resources/mdtest/annotations/literal.md
@@ -5,7 +5,7 @@
 ## Parameterization
 
 ```py
-from typing import Literal
+from typing import Literal, Any
 from enum import Enum
 
 mode: Literal["w", "r"]
@@ -57,9 +57,12 @@ invalid4: Literal[
 
 class NotAnEnum:
     x: int = 1
+    y: list[Any] = []
 
 # error: [invalid-type-form]
 invalid5: Literal[NotAnEnum.x]
+invalid6: Literal[NotAnEnum.y[0]]
+invalid7: Literal[NotAnEnum["x"].value]
 ```
````

---

_Comment by @sharkdp on 2025-07-22 09:36_

> I think "enum member" would be clearer than "enum value" in this error message

:+1:  https://github.com/astral-sh/ruff/pull/19482

---

_Comment by @sharkdp on 2025-07-22 09:41_

> If I apply this diff to your branch then I get some pull-types panics on the `literal.md` mdtest:

I can't see how this would be related to this PR, and in fact, I can also reproduce this on `main`. Will look into it or open a ticket.

---

_Merged by @sharkdp on 2025-07-22 09:42_

---

_Closed by @sharkdp on 2025-07-22 09:42_

---

_Branch deleted on 2025-07-22 09:42_

---
