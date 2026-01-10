---
number: 1775
title: "Add keyword-argument completions when inheriting from `TypedDict`"
type: issue
state: closed
author: AlexWaygood
labels:
  - server
  - completions
assignees: []
created_at: 2025-12-05T14:47:36Z
updated_at: 2025-12-30T13:10:59Z
url: https://github.com/astral-sh/ty/issues/1775
synced_at: 2026-01-10T01:51:14Z
---

# Add keyword-argument completions when inheriting from `TypedDict`

---

_Issue opened by @AlexWaygood on 2025-12-05 14:47_

In this situation, I would expect the first autocomplete suggestion to be `total=`, because you can specify `total=False` when inheriting from `TypedDict` to specify that the `TypedDict` keys in the type you're defining should not be required. Other keyword arguments accepted when subclassing `TypedDict` are `closed` and `extra_items` (see the [spec](https://typing.python.org/en/latest/spec/typeddict.html#class-based-syntax)).

<img width="1456" height="724" alt="Image" src="https://github.com/user-attachments/assets/f26350cd-2776-4b08-8b5d-dd5353fbd675" />

---

_Label `server` added by @AlexWaygood on 2025-12-05 14:47_

---

_Label `completions` added by @AlexWaygood on 2025-12-05 14:47_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-05 15:09_

---

_Closed by @AlexWaygood on 2025-12-30 13:10_

---
