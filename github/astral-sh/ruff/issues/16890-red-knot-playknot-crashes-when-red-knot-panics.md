---
number: 16890
title: "[red-knot] Playknot crashes when Red-knot panics"
type: issue
state: closed
author: cake-monotone
labels:
  - playground
  - ty
assignees: []
created_at: 2025-03-21T09:26:06Z
updated_at: 2025-03-26T18:14:41Z
url: https://github.com/astral-sh/ruff/issues/16890
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Playknot crashes when Red-knot panics

---

_Issue opened by @cake-monotone on 2025-03-21 09:26_

https://playknot.ruff.rs/188faf78-ddb6-4559-8093-79c3a62cd9c2

The issue for red-knot is here: astral-sh/ty#228 

---

_Label `playground` added by @AlexWaygood on 2025-03-21 09:36_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-21 09:36_

---

_Comment by @MichaReiser on 2025-03-21 10:01_

I think the solution here is to fix red knot. :) The only "solution" here would be to catch the exception and then re-initialize the WASM module. The problem with this is that it's very likely that the WASM module will crash again.



---

_Comment by @AlexWaygood on 2025-03-21 10:34_

I agree with @MichaReiser... perhaps in an ideal world the playground would catch any panics raised by red-knot. But in an ideal world, red-knot would never crash in the first place 😄 And fixing that problem seems much more important than making the playground able to catch and recover from red-knot panics

---

_Comment by @MichaReiser on 2025-03-21 10:42_

One thing that *is* annoying is that there's no way to recover the playground when it crashed other than opening the console and clearing the local storage... (or loading a different shared playground)

---

_Comment by @cake-monotone on 2025-03-21 11:00_

Ah! I created the issue just in case. I agree with both of you!
If we have to come up with a reason, I guess it could become a bit of an issue if the playground ends up supporting multi-version Ruff in the future—like a panic happening only in an older version.

---

_Assigned to @MichaReiser by @carljm on 2025-03-26 18:08_

---

_Comment by @MichaReiser on 2025-03-26 18:14_

I added a few more try catch statements that should prevent that the playground crashes entirely but the solution ultimately is to fix red knot

---

_Closed by @MichaReiser on 2025-03-26 18:14_

---
