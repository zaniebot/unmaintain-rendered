---
number: 14737
title: "`OptionSet`: function pointer comparisons do not produce meaningful results since their addresses are not guaranteed to be unique"
type: issue
state: closed
author: konstin
labels:
  - internal
assignees: []
created_at: 2025-07-18T17:44:32Z
updated_at: 2025-07-20T22:17:08Z
url: https://github.com/astral-sh/uv/issues/14737
synced_at: 2026-01-07T13:12:18-06:00
---

# `OptionSet`: function pointer comparisons do not produce meaningful results since their addresses are not guaranteed to be unique

---

_Issue opened by @konstin on 2025-07-18 17:44_

On nightly, clippy notes that `OptionSet` is not sound, as we're doing equally comparisons with it:

```
warning: function pointer comparisons do not produce meaningful results since their addresses are not guaranteed to be unique
  --> crates/uv-options-metadata/src/lib.rs:74:5
   |
72 | #[derive(Copy, Clone, Eq, PartialEq)]
   |                           --------- in this derive macro expansion
73 | pub struct OptionSet {
74 |     record: fn(&mut dyn Visit),
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
   = note: the address of the same function can vary between different codegen units
   = note: furthermore, different functions could have the same address after being merged together
   = note: for more information visit <https://doc.rust-lang.org/nightly/core/ptr/fn.fn_addr_eq.html>
   = note: `#[warn(unpredictable_function_pointer_comparisons)]` on by default
```

It looks like we're only doing those comparisons in tests, though I wouldn't know how to replace them.

---

_Label `internal` added by @konstin on 2025-07-18 17:44_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-20 22:05_

---

_Referenced in [astral-sh/uv#14765](../../astral-sh/uv/pulls/14765.md) on 2025-07-20 22:06_

---

_Closed by @charliermarsh on 2025-07-20 22:17_

---
