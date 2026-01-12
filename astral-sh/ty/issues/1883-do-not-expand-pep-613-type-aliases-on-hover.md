```yaml
number: 1883
title: Do not expand PEP-613 type aliases on hover
type: issue
state: open
author: hamdanal
labels:
  - server
  - type aliases
assignees: []
created_at: 2025-12-14T13:46:02Z
updated_at: 2026-01-11T09:31:07Z
url: https://github.com/astral-sh/ty/issues/1883
synced_at: 2026-01-12T02:26:11Z
```

# Do not expand PEP-613 type aliases on hover

---

_Issue opened by @hamdanal on 2025-12-14 13:46_

They could become unwieldy especially for large unions. See these two examples

<img width="996" height="157" alt="Image" src="https://github.com/user-attachments/assets/be7bb139-2ca2-44e7-86a4-436c2492f01b" />
Where the tuple is defined using a type alias in the stub and

<img width="839" height="281" alt="Image" src="https://github.com/user-attachments/assets/8fdeac9b-426d-4846-b2ca-5b8e941efae8" />

Where the type of `value` is defines as `Scalar | ListLikeU | None` in pandas-stubs.

In both cases displaying the name of the type alias would make the signature more readable. 

---

_Renamed from "Do not expand type aliases on hover" to "Do not expand PEP-613 type aliases on hover" by @AlexWaygood on 2025-12-14 13:47_

---

_Comment by @AlexWaygood on 2025-12-14 13:48_

To implement this, we would need to represent PEP-613 aliases as first-class opaque types in our model, similar to what we do with PEP-695 aliases. (That's definitely something we could do, but it's quite different to our current implementation.)

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-14 13:50_

---

_Label `server` added by @dhruvmanila on 2025-12-15 05:27_

---

_Comment by @carljm on 2025-12-15 19:33_

I think this would pretty much require the refactor to split type-expression inference more fully from value-expression inference.

---

_Label `type aliases` added by @AlexWaygood on 2025-12-19 12:10_

---
