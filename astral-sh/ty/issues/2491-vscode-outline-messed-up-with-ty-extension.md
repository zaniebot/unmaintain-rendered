```yaml
number: 2491
title: VSCode outline messed up with TY extension
type: issue
state: open
author: ErichMarx
labels:
  - question
assignees: []
created_at: 2026-01-14T13:26:28Z
updated_at: 2026-01-14T13:30:38Z
url: https://github.com/astral-sh/ty/issues/2491
synced_at: 2026-01-14T13:42:04Z
```

# VSCode outline messed up with TY extension

---

_@ErichMarx_

### Summary

Before using TY on VSCode, my outline was displayed nicely like this:

<img width="309" height="245" alt="Image" src="https://github.com/user-attachments/assets/5858b132-2fdb-4ad1-b377-74af71b66f9c" />


After installing it, it now shows a higher-level folder for ty and pylance (see below):

<img width="309" height="540" alt="Image" src="https://github.com/user-attachments/assets/767aaeff-4106-4060-b3fa-9c7a37dbe730" />

Though this isn't very disruptive, on bigger files, this isn't as nice. 

### Version

0.0.10

---

_Label `server` added by @AlexWaygood on 2026-01-14 13:28_

---

_Label `server` removed by @MichaReiser on 2026-01-14 13:28_

---

_Label `question` added by @MichaReiser on 2026-01-14 13:28_

---

_Comment by @MichaReiser on 2026-01-14 13:30_

I suspect that you're running both Pylance's and ty's LSP at the same time and VS Code then groups the results by extension. 

Can you double check your [`python.languageServer`](https://code.visualstudio.com/docs/python/settings-reference#_intellisense-engine-settings) setting. Is it set to `None`?

---
