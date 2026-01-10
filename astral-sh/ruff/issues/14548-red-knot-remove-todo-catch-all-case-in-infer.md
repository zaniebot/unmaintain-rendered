---
number: 14548
title: "[red-knot] remove TODO catch-all case in infer_unary_expression"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-11-22T23:30:55Z
updated_at: 2024-12-18T19:37:35Z
url: https://github.com/astral-sh/ruff/issues/14548
synced_at: 2026-01-10T01:22:55Z
---

# [red-knot] remove TODO catch-all case in infer_unary_expression

---

_Issue opened by @carljm on 2024-11-22 23:30_

Currently, the method `TypeInferenceBuilder::infer_unary_expression` handles some common cases, and has a catch-all arm that simply infers a "todo" type.

This issue is to remove that todo catch-all case, and replace it with correct handling of all missing cases.

For many of the unhandled types (e.g. `Type::FunctionLiteral`, `Type::ClassLiteral`, `Type::SubclassOf`) the correct handling will be to treat it as a `Type::Instance` type (an instance of `types.FunctionType`, or an instance of the meta-type of a class) and let the normal instance handling from typeshed take care of it.

(note: edited description for correctness, which will make some comments below look out-of-context)

---

_Label `red-knot` added by @carljm on 2024-11-22 23:30_

---

_Assigned to @carljm by @carljm on 2024-11-22 23:30_

---

_Referenced in [astral-sh/ruff#14549](../../astral-sh/ruff/issues/14549.md) on 2024-11-22 23:34_

---

_Comment by @AlexWaygood on 2024-11-23 10:35_

> For many of the unhandled types (e.g. `Type::FunctionLiteral`, `Type::ClassLiteral`, `Type::SubclassOf`) the correct handling will simply be an `unsupported-operator` diagnostic.

Hmm, I don't think that's correct when it comes to `Type::ClassLiteral` and `Type::SubclassOf`. We need to treat a class as an instance of its metaclass when determining if a unary operation is supported on that class or not. (Same deal as  #14200.)

---

_Comment by @carljm on 2024-11-23 17:08_

Yes, good point! I looked at functions and then thoughtlessly lumped classes in with them. 

Even for functions, we should treat them as an instance of `types.FunctionType` and defer to typeshed, rather than hardcoding the unsupported-operator diagnostic. 

---

_Assigned to @dcreager by @dcreager on 2024-12-17 19:25_

---

_Unassigned @carljm by @dcreager on 2024-12-17 19:25_

---

_Referenced in [astral-sh/ruff#15045](../../astral-sh/ruff/pulls/15045.md) on 2024-12-18 15:48_

---

_Comment by @dcreager on 2024-12-18 19:37_

Closed by #15045 

---

_Closed by @dcreager on 2024-12-18 19:37_

---
