```yaml
number: 185
title: Duplicate diagnostics in unpacking targets
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - diagnostics
assignees: []
created_at: 2025-03-05T04:11:36Z
updated_at: 2025-06-24T02:19:46Z
url: https://github.com/astral-sh/ty/issues/185
synced_at: 2026-01-12T15:54:22Z
```

# Duplicate diagnostics in unpacking targets

---

_@dhruvmanila_

### Summary

Consider the following code:
```py
class Foo():
    pass

a, b = Foo().nonexistant
```

Here, the RHS of the assignment has an error `unresolved-attribute` which is being shown 3 times (2 + 1 where 2 is the number of variables involved in the unpacking):

<img width="1043" alt="Image" src="https://github.com/user-attachments/assets/e6be58cc-cc5e-4079-814c-de25191bc10d" />

This is because the standalone expression is being inferred _outside_ the unpacking in `infer_assignment_definition` (and similarly in `infer_for_statement_definition`). This inference should only be done for non-unpack targets.

Fixing this would lead to another bug which is that the diagnostics raised during the unpacking query isn't being merged into the outer type inference builder.

### Version

_No response_

---

_Label `bug` added by @dhruvmanila on 2025-03-05 04:11_

---

_Renamed from "[red-knot] Duplicate diagnostics in unpacking assignment" to "[red-knot] Duplicate diagnostics in unpacking targets" by @dhruvmanila on 2025-03-05 04:12_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-03-05 04:12_

---

_Unassigned @dhruvmanila by @carljm on 2025-03-27 19:10_

---

_Renamed from "[red-knot] Duplicate diagnostics in unpacking targets" to "Duplicate diagnostics in unpacking targets" by @MichaReiser on 2025-05-07 15:26_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-11 10:53_

---

_Comment by @dhruvmanila on 2025-05-30 08:38_

This should be relatively easy to fix once https://github.com/astral-sh/ruff/pull/18041 has landed. This is because that PR adds a `Definition` ingredient for all attributes which would then allow us to use the `infer_unpack_types` for all attributes involved in an unpacking.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-06-04 06:20_

---

_Comment by @dhruvmanila on 2025-06-13 14:20_

I tried fixing this today based on the assumption that assignments containing an attribute expressions now always create a `Definition` but that isn't true. It's only created if `PlaceExpr::try_from` is successful, so a call expression like the following doesn't create a `Definition`. I'll have to re-think the approach that I used here.

```py
class Foo:
    def __init__(self):
        self.x = 1

Foo().x = 2
```

---

_Closed by @dhruvmanila on 2025-06-24 02:19_

---
