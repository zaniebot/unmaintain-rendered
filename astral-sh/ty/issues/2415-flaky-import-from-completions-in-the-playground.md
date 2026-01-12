```yaml
number: 2415
title: "Flaky(?) `import from` completions in the playground when there are statements below"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - playground
  - completions
assignees: []
created_at: 2026-01-09T13:48:37Z
updated_at: 2026-01-09T21:28:37Z
url: https://github.com/astral-sh/ty/issues/2415
synced_at: 2026-01-12T15:54:26Z
```

# Flaky(?) `import from` completions in the playground when there are statements below

---

_@AlexWaygood_

Consider the following video. `dataclasses` is never suggested as an import at the `from datacl<CURSOR>` location, and at the location `from dataclasses import data<CURSOR>`, the `dataclass` completion suggestion keeps flickering(?) on and off, so fast that I can never actually accept it.

https://github.com/user-attachments/assets/7532bf77-cadc-4a51-b3f4-134648e0f1c1

I don't remember coming across this before -- possibly a recent regression? No idea if this is a general issue or another playground-specific issue, I'm afraid :-)

Cc. @BurntSushi / @MichaReiser 

---

_Label `bug` added by @AlexWaygood on 2026-01-09 13:48_

---

_Label `server` added by @AlexWaygood on 2026-01-09 13:48_

---

_Label `completions` added by @AlexWaygood on 2026-01-09 13:48_

---

_Label `server` removed by @MichaReiser on 2026-01-09 13:51_

---

_Label `playground` added by @MichaReiser on 2026-01-09 13:51_

---

_Comment by @BurntSushi on 2026-01-09 14:06_

Confirming that this doesn't happen in VS Code:

https://github.com/user-attachments/assets/dc15fa9e-93ff-4dc2-94a6-54c4cf970f09

But also seems to be working okay for me in the playground:

https://github.com/user-attachments/assets/16e3e489-1507-41da-9a8f-d888b7184ca1

I guess that's the flaky part?

---

_Comment by @BurntSushi on 2026-01-09 14:07_

Also works okay for me in Chrome.

---

_Comment by @MichaReiser on 2026-01-09 14:11_

I can't reproduce this either. Very possible that there's an issue where we end up re-rendering or updating the editor, which then results in closing/refreshing the completion dropdown

---

_Comment by @MichaReiser on 2026-01-09 14:12_

Or it's some Browser extension that interacts badly

---

_Comment by @AlexWaygood on 2026-01-09 14:18_

This is strange -- it's very persistent for me. I tried clearing my cache for `play.ty.dev` and it still reproduces:

<img width="1286" height="558" alt="Image" src="https://github.com/user-attachments/assets/8c1b4d73-394f-4959-9a49-49e6c3d14a0d" />



---

_Comment by @BurntSushi on 2026-01-09 14:22_

Some other things to try:

1. What happens if, after `from data`, you close the completions and then re-request them with `Ctrl + Space`?
2. Try a different web browser?
3. Try running in an incognito/private window?

---

_Comment by @AlexWaygood on 2026-01-09 14:34_

- It reproduces for me in Firefox when I'm logged into my personal Google account
- It reproduces for me in a private/incognito Firefox window
- It reproduces for me in Google Chrome when I'm logged into my personal Google account
- It reproduces for me in Google Chrome when I'm logged into my Astral Google account
- It reproduces for me in a Google Chrome private/incognito window

`Ctrl + Space` opens up a popup where I can change my keyboard settings from the British layout to the American layout but doesn't seem to re-request completions 😄 Hitting `Esc` then typing `c` at `from data<CURSOR>` does seem to trigger the expected `dataclasses` completion suggestion, however

---

_Comment by @BurntSushi on 2026-01-09 14:54_

> Hitting `Esc` then typing `c` at `from data<CURSOR>` does seem to trigger the expected `dataclasses` completion suggestion, however

This almost seems like you aren't seeing the `isIncomplete` fix I merged in https://github.com/astral-sh/ruff/pull/22442

But that ignores the flaky part.

And I'm baffled that this is reproducing for you across a bunch of different environments. Hmmmmmm.

---

_Comment by @MichaReiser on 2026-01-09 15:02_

Must be a user problem ;)

---

_Comment by @DanielKongsgaard on 2026-01-09 15:08_

I can reproduce this (sometimes). Here is a video with both:

https://github.com/user-attachments/assets/44a119a3-a105-4434-b37c-83bbefbf1fae

https://github.com/user-attachments/assets/1adf140b-6f28-4999-91c9-333e509a0484

At first I thought it might have something to do with the presence of `ty.json`, but now I seems able to reproduce it without the `ty.json` too...

Also, I am a bit confused by the behavior of the `import` completion, which seems to be worse in the first case without the `dataclasses` problem, but better in the case where `dataclasses` doesn't complete nicely.

---

_Comment by @RasmusNygren on 2026-01-09 20:18_

This is consistently broken for me in the playground as well since the `incomplete = True` change. Without that change it works for me.

I can not reproduce this in an editor outside of the playground.

Looking at your video @BurntSushi. For me it looks sane as well when `from d<CURSOR>` is typed, it's only when I type `from da<CURSOR>` that I get the weird behaviour

---

_Comment by @MichaReiser on 2026-01-09 21:28_

I can't observe the flickering but can now reproduce that dataclass isn't always suggested 

---
