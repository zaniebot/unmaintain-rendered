---
number: 2223
title: "Inconsistent type inference for time.perf_counter(): Inferred as int | float instead of float"
type: issue
state: closed
author: Blinkion
labels:
  - question
assignees: []
created_at: 2025-12-26T02:36:05Z
updated_at: 2025-12-26T17:29:47Z
url: https://github.com/astral-sh/ty/issues/2223
synced_at: 2026-01-10T01:52:52Z
---

# Inconsistent type inference for time.perf_counter(): Inferred as int | float instead of float

---

_Issue opened by @Blinkion on 2025-12-26 02:36_

### Summary

Summary
While testing ty, I noticed that the Inlay Hint for time.perf_counter() shows the type as int | float. However, the underlying typeshed stub (time.pyi) bundled with ty explicitly defines the return type as float. Additionally, per PEP 484's "numeric tower" convention, int is a subtype of float, making the int | float union redundant even if an integer were possible.

Environment
ty version: 0.0.7 (cf82a04b5 2025-12-24)

OS: macOS (as seen in screenshots)

Reproduction
Create a Python script:

Python

import time
start = time.perf_counter()
Observe the Inlay Hint for the start variable.

Actual Behavior
The inferred type is displayed as int | float.

Expected Behavior
The inferred type should be float, strictly matching the typeshed definition: def perf_counter() -> float: ...

Additional Context
I've verified the vendored typeshed file within ty's cache (.cache/ty/vendored/typeshed/.../time.pyi) defines the return type as float.

In the context of static analysis, float should be sufficient as it encompasses int.

<img width="576" height="212" alt="Image" src="https://github.com/user-attachments/assets/2c5600e5-9390-4e70-822d-15e2b04f5d24" />
<img width="827" height="203" alt="Image" src="https://github.com/user-attachments/assets/14c04426-d28f-4b23-aa85-eff0aba3fe96" />

### Version

0.0.7 (cf82a04b5 2025-12-24)

---

_Comment by @Wizzerinus on 2025-12-26 08:09_

I believe ty explicitly chose to use int | float for definitions because int isn't actually liskov-compatible with float. See here: https://docs.astral.sh/ty/reference/typing-faq/#why-does-ty-show-int-float-when-i-annotate-something-as-float

---

_Comment by @Blinkion on 2025-12-26 08:09_

这是来自QQ邮箱的假期自动回复邮件。您好，我已经收到您的邮件。我将在阅读后，尽快给您回复。

---

_Label `question` added by @MichaReiser on 2025-12-26 08:40_

---

_Closed by @carljm on 2025-12-26 17:29_

---
