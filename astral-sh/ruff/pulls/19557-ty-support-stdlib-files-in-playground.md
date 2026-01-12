```yaml
number: 19557
title: "[ty] Support stdlib files in playground"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: fix-vendored-file-back-button
created_at: 2025-07-25T14:08:14Z
updated_at: 2025-07-26T18:33:40Z
url: https://github.com/astral-sh/ruff/pull/19557
synced_at: 2026-01-12T15:56:42Z
```

# [ty] Support stdlib files in playground

---

_@MichaReiser_

## Summary

Adds support for goto-* for builtins to the playground


https://github.com/user-attachments/assets/c9cabbcf-e043-4160-be17-5156d653984e

It's not the most elegant approach with the inline banner but it works pretty well and has the benefit that it doesn't complicate the Playground as much as if we would need to integrate this into the tabs navigation.

Closes https://github.com/astral-sh/ty/issues/751

---

_Label `playground` added by @MichaReiser on 2025-07-25 14:08_

---

_Label `ty` added by @MichaReiser on 2025-07-25 14:08_

---

_Comment by @github-actions[bot] on 2025-07-25 14:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-07-25 17:10_

---

_Review requested from @carljm by @MichaReiser on 2025-07-25 17:10_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-25 17:10_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-25 17:10_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-25 17:10_

---

_Comment by @carljm on 2025-07-25 17:23_

ty playground continues on its inevitable march towards being a full-fledged web-based IDE...

---

_Comment by @AlexWaygood on 2025-07-25 17:41_

This is awesome!! A few _tiny_ nits from playing around with it locally (none blocking):

- Would it be possible to add a key binding so that pressing "Esc." exits the file-within-a-file?
- It seems like the "Diagnostics" panel can't be resized when you're looking at at vendored file -- is that expected? It's a bit annoying if you had it huge before you jumped to the vendored file and now you can't resize it without jumping out of the vendored file again 😄
- It looks like the "vendored files cannot be run" message is displayed in a black font in the "run" tab even if you're in dark mode, which doesn't work great ;-)
<img width="3840" height="1854" alt="image" src="https://github.com/user-attachments/assets/7b7edda4-2e5f-402c-b8f3-2a6e1dc2ad9e" />

- It surprised me a bit here that `tuple` was towards the bottom of my screen after I jumped to its definition and the file-within-a-file opened up -- I'd expect `tuple` to be right at the top after jumping to its definition:

https://github.com/user-attachments/assets/f1579b15-9794-419a-9a70-9950a99c7609





---

_Comment by @MichaReiser on 2025-07-25 17:49_

> It surprised me a bit here that tuple was towards the bottom of my screen after I jumped to its definition and the file-within-a-file opened up -- I'd expect tuple to be right at the top after jumping to its definition:

That's monaco. I don't think I can do anything about that

> Would it be possible to add a key binding so that pressing "Esc." exits the file-within-a-file?

If claude can figure it out in 1min, otherwise no

---

_Renamed from "[ty] Support builtin files in playground" to "[ty] Support stdlib files in playground" by @AlexWaygood on 2025-07-25 18:08_

---

_Comment by @MichaReiser on 2025-07-26 11:38_

Thanks for the extensive testing. I addressed all the feedback

---

_@AlexWaygood approved on 2025-07-26 11:39_

I haven't reviewed the TypeScript (I'm definitely not a frontend developer 😆) but the UX seems fantastic! Thanks for making the updates 😃

---

_Comment by @MichaReiser on 2025-07-26 11:41_

Claude wrote the code and I reviewed (and deleted about 50% that was unnecessary) 😅 

---

_Merged by @MichaReiser on 2025-07-26 18:33_

---

_Closed by @MichaReiser on 2025-07-26 18:33_

---

_Branch deleted on 2025-07-26 18:33_

---
