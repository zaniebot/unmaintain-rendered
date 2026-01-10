```yaml
number: 13725
title: "[red-knot] Use colors to improve readability of `mdtest` output"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: mdtest-colours
created_at: 2024-10-12T22:52:43Z
updated_at: 2024-10-13T13:20:37Z
url: https://github.com/astral-sh/ruff/pull/13725
synced_at: 2026-01-10T20:59:36Z
```

# [red-knot] Use colors to improve readability of `mdtest` output

---

_Pull request opened by @AlexWaygood on 2024-10-12 22:52_

## Summary

Screenshot of previous output when tests failed:

![image](https://github.com/user-attachments/assets/33c6c126-f173-496f-97fd-db7a1591ada8)

---

With this PR:

![image](https://github.com/user-attachments/assets/7c079351-098c-41de-8a03-0f21c120eee0)

## Test Plan

I applied this diff to the branch for this PR, and ran `cargo test -p red_knot_python_semantic`, then took the above screenshots:

````diff
--- a/crates/red_knot_python_semantic/resources/mdtest/numbers.md
+++ b/crates/red_knot_python_semantic/resources/mdtest/numbers.md
@@ -23,7 +23,7 @@ reveal_type(9223372036854775808)  # revealed: int
 There aren't literal float types, but we infer the general float type:
 
 ```py
-reveal_type(1.0)  # revealed: float
+reveal_type(1.0)  # revealed: str
 ```
 
 ## Complex
@@ -31,5 +31,5 @@ reveal_type(1.0)  # revealed: float
 Same for complex:
 
 ```py
-reveal_type(2j)  # revealed: complex
+reveal_type(2j)
 ```
````


---

_Label `testing` added by @AlexWaygood on 2024-10-12 22:52_

---

_Label `red-knot` added by @AlexWaygood on 2024-10-12 22:52_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-12 22:52_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-12 22:52_

---

_Comment by @github-actions[bot] on 2024-10-12 23:13_

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

_@carljm approved on 2024-10-13 03:36_

Looks good to me! Thank you!

---

_Comment by @MichaReiser on 2024-10-13 07:06_

I think I would prefer to highlight part of the line instead of the entire line because the colors now become distracting (it's too colourful)

---

_Comment by @AlexWaygood on 2024-10-13 10:52_

> I think I would prefer to highlight part of the line instead of the entire line because the colors now become distracting (it's too colourful)

I pushed some changes; how does it look to you now? Screenshot of what the output now looks like:

![image](https://github.com/user-attachments/assets/9e8f3859-f452-4561-bc69-dc76e30c6fc2)


---

_Comment by @MichaReiser on 2024-10-13 13:19_

That looks much better to me. Thanks! 

---

_Merged by @AlexWaygood on 2024-10-13 13:20_

---

_Closed by @AlexWaygood on 2024-10-13 13:20_

---

_Branch deleted on 2024-10-13 13:20_

---
