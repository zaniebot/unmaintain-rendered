---
number: 144
title: Goto-type and hover-type for non-expression nodes (identifiers in statements)
type: issue
state: open
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-04-01T10:55:10Z
updated_at: 2025-12-31T16:37:53Z
url: https://github.com/astral-sh/ty/issues/144
synced_at: 2026-01-10T01:51:14Z
---

# Goto-type and hover-type for non-expression nodes (identifiers in statements)

---

_Issue opened by @MichaReiser on 2025-04-01 10:55_

https://github.com/astral-sh/ruff/pull/16901 implemented basic goto type definition support. However, there are still a handful of nodes that have identifier children for which goto type definition doesn't work because the enclosing node isn't an expression or a definition. We should add support for goto type definition for those nodes as well. 

See the TODO comments in `ty_ide::goto::GotoTarget::inferred_type` 

Status:

* [x] goto-declaration
* [x] goto-definition
* [x] find-references
* [x] rename (caveat: https://github.com/astral-sh/ty/issues/1661) 
* [x] hover (caveat: we do not display the type on hover, limited by goto-type)
* [ ] goto-type
  * [x] imports
  * [ ] global/nonlocal - needs type inference changes to compute+record the type
  * [ ] match patterns - needs type inference changes to compute+record the type
  * [ ] type params (? should we even do anything here) 

---

_Renamed from "[red-knot] Goto type definition for non-expression nodes" to "Goto type definition for non-expression nodes" by @MichaReiser on 2025-05-07 15:25_

---

_Label `server` added by @AlexWaygood on 2025-05-11 07:54_

---

_Comment by @UnboundVariable on 2025-07-24 20:01_

This same TODO affects the "goto declaration" and "goto definition" features. Once addressed, all three "goto" features should support these non-expression nodes.

---

_Removed from milestone `GA` by @MichaReiser on 2025-08-15 07:04_

---

_Added to milestone `Beta` by @MichaReiser on 2025-08-15 07:04_

---

_Assigned to @Gankra by @carljm on 2025-08-15 15:34_

---

_Removed from milestone `Beta` by @carljm on 2025-08-15 15:34_

---

_Added to milestone `GA` by @carljm on 2025-08-15 15:34_

---

_Renamed from "Goto type definition for non-expression nodes" to "Goto-type and hover-type for non-expression nodes (identifiers in statements)" by @Gankra on 2025-12-02 19:47_

---

_Comment by @Gankra on 2025-12-02 19:52_

I sketched out the IDE half of this here: https://github.com/astral-sh/ruff/pull/21762

---

_Unassigned @Gankra by @Gankra on 2025-12-02 19:54_

---

_Comment by @Gankra on 2025-12-02 19:55_

I'm going to unassign myself from this issue because I've pushed this functionality as far as I'm going to.

---

_Removed from milestone `Stable` by @MichaReiser on 2025-12-31 16:37_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 16:37_

---

_Removed from milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 16:37_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-31 16:37_

---
