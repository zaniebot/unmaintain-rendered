```yaml
number: 1081
title: No completion of methods and attributes inside of class definition
type: issue
state: closed
author: JaRoSchm
labels:
  - server
  - generics
  - typing semantics
  - completions
assignees: []
created_at: 2025-08-21T11:43:12Z
updated_at: 2025-10-23T08:02:54Z
url: https://github.com/astral-sh/ty/issues/1081
synced_at: 2026-01-10T02:06:24Z
```

# No completion of methods and attributes inside of class definition

---

_Issue opened by @JaRoSchm on 2025-08-21 11:43_

### Summary

Hi, the completion of methods and attributes does only work outside of the class definition but not inside of it. Here everything works as expected:

<img width="254" height="304" alt="Image" src="https://github.com/user-attachments/assets/a32ea72c-20e9-4336-8789-d4e68747e214" />
<img width="274" height="303" alt="Image" src="https://github.com/user-attachments/assets/5f84549b-0fa5-4824-bf8b-001ac9f16402" />

While inside of the class there are no completion suggestion at all:

<img width="254" height="187" alt="Image" src="https://github.com/user-attachments/assets/c53cef73-5412-4758-b171-65da3a0e5c82" />

### Version

ty 0.0.1-alpha.19

---

_Comment by @MichaReiser on 2025-08-21 11:50_

I'd have to double check but I'm fairly certain that this is due to ty not supporting `self` yet. 

https://github.com/astral-sh/ty/issues/159

---

_Label `server` added by @MichaReiser on 2025-08-21 11:50_

---

_Comment by @JaRoSchm on 2025-08-21 11:58_

Thanks, should I then close this because it is expected to be resolved automatically?

---

_Comment by @MichaReiser on 2025-08-21 12:02_

I'd keep it open for now. It's easier to find for users who run into the same issue as you.

---

_Comment by @anthony2261 on 2025-08-26 08:22_

Yup, same situation here! Thanks for opening the issue, good to know that this is documented

---

_Label `generics` added by @BurntSushi on 2025-08-26 12:03_

---

_Label `typing semantics` added by @BurntSushi on 2025-08-26 12:03_

---

_Comment by @godsplanhub on 2025-09-03 13:20_

Yes also no autocompletions for pydantic Models or SQLModels  hope this will be resolved

---

_Label `completions` added by @AlexWaygood on 2025-09-30 15:49_

---

_Closed by @sharkdp on 2025-10-23 07:34_

---

_Comment by @JaRoSchm on 2025-10-23 08:02_

Thank you!

---
