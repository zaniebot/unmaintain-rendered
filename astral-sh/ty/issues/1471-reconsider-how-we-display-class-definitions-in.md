```yaml
number: 1471
title: Reconsider how we display class definitions in the LSP
type: issue
state: open
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-11-03T14:23:43Z
updated_at: 2026-01-03T17:46:45Z
url: https://github.com/astral-sh/ty/issues/1471
synced_at: 2026-01-12T15:54:25Z
```

# Reconsider how we display class definitions in the LSP

---

_@MichaReiser_

> Most classes and types will have lsp description of <class 'Foo'> (which is especially weird for built-in ones like <class 'int'>) and doesn't seem to carry much info


Pylance:

https://github.com/user-attachments/assets/ad50fd03-70b5-485d-a5fe-0d7e41c1f68f

ty

https://github.com/user-attachments/assets/02a7ea6e-2c40-45db-abaa-454be4971204


Both ty and pylance show that the type is a class, but IMO, pylance does so in a more idiomatic way in the LSP by using `class <TYPE>` over `<class <TYPE>>`. 

We should consider changing how we render class definitions within the LSP (across operations)

---

_Label `help wanted` added by @MichaReiser on 2025-11-03 14:23_

---

_Label `server` added by @MichaReiser on 2025-11-03 14:23_

---

_Renamed from "Reconsider how we display class definitions" to "Reconsider how we display class definitions in the LSP" by @AlexWaygood on 2025-11-03 14:30_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:22_

---

_Comment by @sjpaige on 2026-01-03 00:58_

Can I look into this if no one else has taken it on? 

---

_Comment by @MichaReiser on 2026-01-03 17:40_

I don't think this is a great first contribution issue because it will require some upfront design work before opening a PR, because this might also touch on how we show classes in type signatures.

---

_Label `help wanted` removed by @AlexWaygood on 2026-01-03 17:41_

---

_Comment by @sjpaige on 2026-01-03 17:46_

@MichaReiser No worries! Once I started following the code I figured there would be a bigger conversation around it but wanted to take a stab anyways. But makes sense you wouldn't want someone new/lower context to propose those designs.

---
