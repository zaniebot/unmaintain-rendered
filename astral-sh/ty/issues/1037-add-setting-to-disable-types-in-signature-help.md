```yaml
number: 1037
title: Add setting to disable types in signature help
type: issue
state: open
author: JaRoSchm
labels:
  - configuration
  - needs-decision
  - server
assignees: []
created_at: 2025-08-18T07:54:08Z
updated_at: 2025-11-18T16:10:36Z
url: https://github.com/astral-sh/ty/issues/1037
synced_at: 2026-01-10T01:58:59Z
```

# Add setting to disable types in signature help

---

_Issue opened by @JaRoSchm on 2025-08-18 07:54_

Hi, for me the types displayed in the signature in the hover popup or the completion of function parameters are often not helpful. Even if the types would be formatted better (https://github.com/astral-sh/ty/issues/1000), they would still reduce the readability of the important signature without having a substantial advantage for me. Often, this is due to the typing language of python getting more and more complicated (see for example the screenshot below, what is `SupportsIndex` supposed to mean?). This problem is especially large in the scientific python ecosystem, where either the name of the parameter is enough to directly understand what is needed or so complicated that you have to read the documentation and understand the underlying algorithm where the type is simply not enough. Therefore, I would suggest to add an option to not show types there.

Here are two exemplary screenshots:

<img width="1512" height="613" alt="Image" src="https://github.com/user-attachments/assets/a5a2d8c1-37d4-444a-9919-b9a37813b43d" />
<img width="1893" height="668" alt="Image" src="https://github.com/user-attachments/assets/0384be6f-9ea1-472d-9cee-85f017c3c625" />

---

_Label `server` added by @AlexWaygood on 2025-08-18 08:28_

---

_Label `configuration` added by @dhruvmanila on 2025-08-18 08:47_

---

_Label `needs-decision` added by @MichaReiser on 2025-08-18 10:33_

---

_Added to milestone `Z post-stable` by @MichaReiser on 2025-11-14 09:02_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
