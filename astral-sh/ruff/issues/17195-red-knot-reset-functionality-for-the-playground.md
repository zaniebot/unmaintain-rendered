---
number: 17195
title: "[red-knot] 'Reset' functionality for the playground"
type: issue
state: closed
author: sharkdp
labels:
  - playground
assignees: []
created_at: 2025-04-04T08:11:36Z
updated_at: 2025-04-06T17:09:58Z
url: https://github.com/astral-sh/ruff/issues/17195
synced_at: 2026-01-10T01:22:58Z
---

# [red-knot] 'Reset' functionality for the playground

---

_Issue opened by @sharkdp on 2025-04-04 08:11_

It would be fantastic if the playground had a "reset" button. I often close the `knot.json` tab and then I need to manually recreate it or go to the Dev tools and clear local cache + reload the page.

I tried to quickly implement this myself, but that turned out to be a bad idea, because it turns out calling `localStorage.clear();` is not enough. And beyond that, I have no idea what I'm doing. Well, at least it looked great:

![Image](https://github.com/user-attachments/assets/d7b5f327-f579-413f-99d9-ca771ad5ba91)

---

_Label `playground` added by @sharkdp on 2025-04-04 08:11_

---

_Comment by @MichaReiser on 2025-04-04 08:47_

This is a great idea. I'm not sure where to place the *Reset* button. I don't want to put it into the header because it then breaks the mobile layout (which, admittedly isn't great but it looks reasonable)

Other options are to either add it to the tabs bar or add it as an item in the editor context menu (hard to discover, has the wrong scope)

---

_Comment by @MichaReiser on 2025-04-04 08:48_

I guess, an alternative is to put it into the header but hide it on mobile. Not great but I don't think anyone's writing playgrounds on mobile anyway

---

_Comment by @AlexWaygood on 2025-04-04 11:21_

> but I don't think anyone's writing playgrounds on mobile anyway

I actually did this quite a lot with mypy-playground, which is quite mobile-friendly: it was a great way to triage mypy issues while travelling. But it's quite hard to write code in the Ruff and red-knot playgrounds, so I generally don't use them at all on my phone right now

---

_Referenced in [astral-sh/ruff#17236](../../astral-sh/ruff/pulls/17236.md) on 2025-04-06 17:00_

---

_Closed by @MichaReiser on 2025-04-06 17:09_

---
