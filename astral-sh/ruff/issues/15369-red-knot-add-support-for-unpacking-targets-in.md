```yaml
number: 15369
title: "[red-knot] Add support for unpacking targets in comprehensions"
type: issue
state: closed
author: dhruvmanila
labels:
  - ty
assignees: []
created_at: 2025-01-09T08:55:31Z
updated_at: 2025-04-19T01:12:50Z
url: https://github.com/astral-sh/ruff/issues/15369
synced_at: 2026-01-10T11:09:56Z
```

# [red-knot] Add support for unpacking targets in comprehensions

---

_Issue opened by @dhruvmanila on 2025-01-09 08:55_

This should be very similar to what was done in the `for` statement (https://github.com/astral-sh/ruff/pull/15058). 

**Notes:**

One difference between the `for` statement and comprehension is that the type of value expression needs to be inferred from different scope for the first comprehension. This could be done by extending the `UnpackValue` enum to include a `Comprehension` variant with a `is_first` boolean field.

I'm worried that this logic (https://github.com/astral-sh/ruff/blob/97d95da3ed79a01d780279c4472edc5b9cd357eb/crates/red_knot_python_semantic/src/types/infer.rs#L2964-L2980) would need to be duplicated (1) in `infer_comprehension_definition` when the target is just a name expression and (2) in `Unpacker::unpack` to get the unpack value type from the correct scope.

---

_Label `red-knot` added by @dhruvmanila on 2025-01-09 08:55_

---

_Comment by @dhruvmanila on 2025-01-09 08:57_

I thought this might be "help wanted" issue but it might be a bit difficult without have the relevant context but if anyone's interested in tackling this I'm happy to help.

---

_Added to milestone `Red Knot Beta` by @carljm on 2025-03-27 18:34_

---

_Closed by @dhruvmanila on 2025-04-19 01:12_

---

_Closed by @dhruvmanila on 2025-04-19 01:12_

---
