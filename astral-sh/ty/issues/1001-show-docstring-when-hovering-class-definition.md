```yaml
number: 1001
title: Show docstring when hovering class definition
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-08-15T12:46:27Z
updated_at: 2025-08-19T01:42:55Z
url: https://github.com/astral-sh/ty/issues/1001
synced_at: 2026-01-12T15:54:24Z
```

# Show docstring when hovering class definition

---

_@MichaReiser_

The class's docstring is shown when hovering a reference but not when hovering the definition itself.

https://github.com/user-attachments/assets/ec1efce1-b42f-49d3-a243-86a4b3c8d863

CC: @Gankra I believe you're aware of it but I didn't find a tracking issue

---

_Added to milestone `Beta` by @MichaReiser on 2025-08-15 12:46_

---

_Label `server` added by @MichaReiser on 2025-08-15 12:46_

---

_Comment by @AlexWaygood on 2025-08-15 12:55_

Why is that desirable? The docstring is right there beneath the class definition already 😆

---

_Comment by @MichaReiser on 2025-08-15 13:06_

It's consistent with other LSPs but also feels intuitive, I expect that hovering `PythonCodeTextSplitter` anywhere in a file (assuming it isn't redeclared or a constructor call) shows the same information.

I also use this often to validate that my documentation renders correctly

---

_Comment by @AlexWaygood on 2025-08-15 13:07_

Fair enough!

---

_Comment by @Gankra on 2025-08-15 13:18_

The fix is straight-forward, and lies in the reason why I introduced DefinitionsOrTargets in goto -- there's several kinds of GotoTarget where we never lookup its definition because we're like "oh it's right there" and directly emit the NavigationTarget. If we change those cases to return Definitions we'll start getting their docs:

https://github.com/astral-sh/ruff/blob/bd4506aac56228af83d8ad76569530b3b32be473/crates/ty_ide/src/goto.rs#L298-L307

---

_Comment by @Gankra on 2025-08-15 13:18_

(This issue also exists for function definitions)

---

_Comment by @Gankra on 2025-08-15 14:12_

One non-trivial reason to care about this: if the LSP does a good job of rendering the docstring. It will be more readable and will potentially have clickable links.

---

_Assigned to @Gankra by @carljm on 2025-08-15 15:32_

---

_Closed by @Gankra on 2025-08-19 01:42_

---
