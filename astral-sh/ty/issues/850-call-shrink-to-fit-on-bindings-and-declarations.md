```yaml
number: 850
title: "Call `shrink_to_fit` on `Bindings` and `Declarations` in `UseDefMap`"
type: issue
state: closed
author: MichaReiser
labels:
  - help wanted
  - memory
assignees: []
created_at: 2025-07-18T06:07:59Z
updated_at: 2025-07-22T12:31:17Z
url: https://github.com/astral-sh/ty/issues/850
synced_at: 2026-01-12T15:54:24Z
```

# Call `shrink_to_fit` on `Bindings` and `Declarations` in `UseDefMap`

---

_@MichaReiser_

The `UseDefMap` is the most dominant data structure when it comes to memory usage. We should explore if calling `shrink_to_fit` on `Declarations` and `Bindings` (and all the types that wrap either of those like `PlaceState`) help reduce overall memory consumption.

---

_Label `help wanted` added by @MichaReiser on 2025-07-18 06:08_

---

_Label `memory` added by @MichaReiser on 2025-07-18 06:08_

---

_Comment by @MichaReiser on 2025-07-22 12:31_

I think this has now been implemented

---

_Closed by @MichaReiser on 2025-07-22 12:31_

---
