```yaml
number: 907
title: "Empty preview window when jumping to definition of `dataclasses.field`"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - playground
assignees: []
created_at: 2025-07-28T06:59:27Z
updated_at: 2025-07-28T08:13:01Z
url: https://github.com/astral-sh/ty/issues/907
synced_at: 2026-01-12T15:54:24Z
```

# Empty preview window when jumping to definition of `dataclasses.field`

---

_@sharkdp_

Go to https://play.ty.dev/a9c085a2-f7d2-4998-8bc5-4540c5c3d826 and ctrl-click on `field`. It shows empty previews:

<img width="1208" height="566" alt="Image" src="https://github.com/user-attachments/assets/717094ea-4a57-4a34-a02a-98fc151ce3de" />

Interestingly, after I jumped to the `dataclasses` module first, it seems to work fine (although I noticed that it jumps to a 3.14 overload which should not be reachable on the default configuration)

<img width="1581" height="577" alt="Image" src="https://github.com/user-attachments/assets/13609c02-2784-423e-9764-2f780fb7a519" />

---

_Label `playground` added by @sharkdp on 2025-07-28 06:59_

---

_Comment by @MichaReiser on 2025-07-28 07:03_

Hmm, that might be trickier to integrate. It's not entirely clear what the right entry point here is. I suspect that the playground has the same issue for regular files after loading a stored playground and you haven't opened those files yet

> Interestingly, after I jumped to the dataclasses module first, it seems to work fine (although I noticed that it jumps to a 3.14 overload which should not be reachable on the default configuration)

That's more likely a bug with go to definition itself

---

_Label `bug` added by @MichaReiser on 2025-07-28 07:06_

---

_Closed by @MichaReiser on 2025-07-28 08:13_

---
