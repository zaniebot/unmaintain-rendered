```yaml
number: 495
title: "Reduce size of `TypeInference`"
type: issue
state: closed
author: MichaReiser
labels:
  - memory
assignees: []
created_at: 2025-05-23T13:03:00Z
updated_at: 2025-07-22T09:36:38Z
url: https://github.com/astral-sh/ty/issues/495
synced_at: 2026-01-10T02:06:24Z
```

# Reduce size of `TypeInference`

---

_Issue opened by @MichaReiser on 2025-05-23 13:03_

ty creates a lot of `TypeInference` instances and it is a very large struct. It creates an instance for every:

* scope
* definition
* standalone expression

We should explore if:

* if we can reduce the size of `TypeInference` itself (e.g. use of `ThinVec`?)
* if we can reduce the size of `TypeInference` for standalone expressions, or `definition` or do they use all fields?



---

_Label `memory` added by @MichaReiser on 2025-05-23 13:03_

---

_Comment by @MichaReiser on 2025-05-23 13:03_

CC: @ibraheemdev this might be less effort than the AST improvements. In case you're looking for something smaller on the side

---

_Comment by @monoid on 2025-05-28 12:21_

Do you mean ruff's `TypeInference`?

---

_Comment by @MichaReiser on 2025-05-28 12:22_

> Do you mean ruff's `TypeInference`?

Can you say more. I don't understand the question 

---

_Comment by @monoid on 2025-05-28 12:25_

> > Do you mean ruff's `TypeInference`?
> 
> Can you say more. I don't understand the question

There is a `TypeInference` in the ruff sources: https://github.com/astral-sh/ruff/blob/a3ee6bb3b5f4f375e606d3bc12094549bb46a98b/crates/ty_python_semantic/src/types/infer.rs#L395

and I cannot grep another `TypeInference` in ty sources.

---

_Comment by @MichaReiser on 2025-05-28 12:29_

Yes, that's the type

---

_Comment by @carljm on 2025-05-28 14:44_

@monoid All of ty's source code is in the ruff repository, in the ty crates. This is to simplify code sharing between ruff and ty.

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:47_

---

_Closed by @MichaReiser on 2025-07-22 09:36_

---
