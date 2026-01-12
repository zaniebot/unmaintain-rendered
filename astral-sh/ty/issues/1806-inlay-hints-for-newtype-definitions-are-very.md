```yaml
number: 1806
title: "Inlay hints for `NewType` definitions are very verbose and somewhat fatuous"
type: issue
state: open
author: AlexWaygood
labels:
  - server
assignees: []
created_at: 2025-12-08T12:56:11Z
updated_at: 2025-12-08T16:07:35Z
url: https://github.com/astral-sh/ty/issues/1806
synced_at: 2026-01-12T15:54:25Z
```

# Inlay hints for `NewType` definitions are very verbose and somewhat fatuous

---

_@AlexWaygood_

It feels like the inlay hint here is just repeating exactly the same information that can be gleaned without any inlay:

<img width="1978" height="234" alt="Image" src="https://github.com/user-attachments/assets/4b2b52b8-a84c-48de-b0d8-bfc09def9492" />

(The file has `from typing import NewType` at the top of the file.)

Cc. @Gankra for inlays

---

_Label `server` added by @AlexWaygood on 2025-12-08 12:56_

---

_Comment by @AlexWaygood on 2025-12-08 12:57_

This one also feels a bit fatuous, though I'm guessing it's harder to fix:

<img width="1392" height="126" alt="Image" src="https://github.com/user-attachments/assets/1db03286-2507-46ba-a45a-e376defb1a96" />

---

_Comment by @Gankra on 2025-12-08 15:56_

I am constantly wondering if we should discard any inlay hint that's Literal, and it sure would cover that second case...

---

_Comment by @Gankra on 2025-12-08 15:58_

I have more mixed feelings about `<NewType pseudo-class>`, because I think it's exceedingly rare to ever make a NewType and it might be genuinely helpful to have a flag raised that "hey this is a wild thing".

---

_Comment by @AlexWaygood on 2025-12-08 16:00_

but you know what you're doing if you _are_ making them, right? And if you're a NewType fan (apparently I am, I guess 😆) then you might make [three in a row](https://github.com/astral-sh/ruff/blob/0ccd84136a32f2ab1833b34e5e6128ad4525432f/python/py-fuzzer/fuzz.py#L43-L45).

---

_Comment by @AlexWaygood on 2025-12-08 16:01_

it just doesn't feel like there's any ambiguity there about what the assignment might do, to me :-)

---

_Comment by @MichaReiser on 2025-12-08 16:07_

> but you know what you're doing if you _are_ making them, right? And if you're a NewType fan (apparently I am, I guess 😆) then you might make [three in a row](https://github.com/astral-sh/ruff/blob/0ccd84136a32f2ab1833b34e5e6128ad4525432f/python/py-fuzzer/fuzz.py#L43-L45).

Or you just work with someone who knows what they're doing, but you have to understand their code ;)

---
