```yaml
number: 1542
title: "Autocomplete suggestions do not include a dataclass's generated `__replace__` method"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - server
  - dataclasses
  - completions
assignees: []
created_at: 2025-11-13T20:30:41Z
updated_at: 2025-11-14T10:31:21Z
url: https://github.com/astral-sh/ty/issues/1542
synced_at: 2026-01-12T15:54:25Z
```

# Autocomplete suggestions do not include a dataclass's generated `__replace__` method

---

_@AlexWaygood_

### Summary

I'd expect a `__replace__` autocomplete suggestion here (once I finish typing it out manually, ty is aware that it's an available attribute on `Foo` and infers the type correctly):

<img width="860" height="434" alt="Image" src="https://github.com/user-attachments/assets/d4d5eec5-877c-4233-8821-4594da84f8eb" />

https://play.ty.dev/e4534ec1-dfca-4510-ad95-f938dbf56713

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-11-13 20:30_

---

_Label `server` added by @AlexWaygood on 2025-11-13 20:30_

---

_Label `dataclasses` added by @AlexWaygood on 2025-11-13 20:30_

---

_Label `completions` added by @AlexWaygood on 2025-11-13 20:30_

---

_Comment by @MatthewMckee4 on 2025-11-13 23:25_

I'm not sure what to make of this, but in this specific situation it kinda works.

<img width="865" height="376" alt="Image" src="https://github.com/user-attachments/assets/28d8070a-03a5-48a2-a130-7ecc94709232" />

But not here.

<img width="865" height="376" alt="Image" src="https://github.com/user-attachments/assets/99427b61-535a-4093-a6e0-b674f364d5ce" />

I am not sure why having this line at the bottom seems to have an effect.

---

_Comment by @sharkdp on 2025-11-14 07:46_

If it shows a "abc" symbol in the dropdown, that's just a "I found this word in this document" completion (which comes from Monaco directly?), not a semantic completion that ty provides. The cue could even just be in a comment:

<img width="494" height="134" alt="Image" src="https://github.com/user-attachments/assets/9bc768f7-dbc8-4140-86c3-c114d8e30935" />

---

_Assigned to @sharkdp by @sharkdp on 2025-11-14 07:47_

---

_Closed by @sharkdp on 2025-11-14 10:31_

---
