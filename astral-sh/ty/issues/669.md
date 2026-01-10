```yaml
number: 669
title: Use specialized parameter type for overload step 5 filtering
type: issue
state: closed
author: dhruvmanila
labels:
  - generics
  - overloads
assignees: []
created_at: 2025-06-17T09:32:01Z
updated_at: 2025-08-20T04:09:06Z
url: https://github.com/astral-sh/ty/issues/669
synced_at: 2026-01-10T02:06:24Z
```

# Use specialized parameter type for overload step 5 filtering

---

_Issue opened by @dhruvmanila on 2025-06-17 09:32_

Reference: https://github.com/astral-sh/ruff/pull/18607#discussion_r2151140469

Resolve this TODO: https://github.com/astral-sh/ruff/blob/aa8bbd0be7c04a8ee9347acbec63954b189cf034/crates/ty_python_semantic/resources/mdtest/call/overloads.md?plain=1#L624-L625

---

_Label `overloads` added by @dhruvmanila on 2025-06-17 09:32_

---

_Label `generics` added by @carljm on 2025-06-17 15:27_

---

_Comment by @dhruvmanila on 2025-06-26 03:29_

This is a bit hand-wavy but I think what we might need to do here is to separate out the specialization building part when performing type checking to a different method like `with_specialization` i.e., the following piece of code:

https://github.com/astral-sh/ruff/blob/4a5715b97aed71a8682d62143dd18d46895ab861/crates/ty_python_semantic/src/types/call/bind.rs#L1935-L1970

Then,
- Build up the specializations for all the overloads that are involved in step 5 of call evaluation algorithm i.e., `filter_overloads_using_any_or_unknown` method
- Apply this specialization to the parameter types here

https://github.com/astral-sh/ruff/blob/4a5715b97aed71a8682d62143dd18d46895ab861/crates/ty_python_semantic/src/types/call/bind.rs#L1423-L1428

So, that the assignability check between the top materialized argument types and the union of parameter types works.

---

_Comment by @abhijeetbodas2001 on 2025-07-13 07:36_

@dhruvmanila can I work on this?

---

_Comment by @dhruvmanila on 2025-07-14 06:34_

> can I work on this?

Sure, that would be great! Feel free to ping me if there's any issue that you run into.

---

_Assigned to @abhijeetbodas2001 by @dhruvmanila on 2025-07-14 06:34_

---

_Unassigned @abhijeetbodas2001 by @dhruvmanila on 2025-07-24 08:53_

---

_Added to milestone `Beta` by @carljm on 2025-07-25 14:39_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-08-08 10:28_

---

_Closed by @dhruvmanila on 2025-08-20 04:09_

---
