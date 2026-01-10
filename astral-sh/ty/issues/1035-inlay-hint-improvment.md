```yaml
number: 1035
title: Inlay Hint Improvment
type: issue
state: closed
author: alirezanobakht13
labels:
  - server
  - wish
assignees: []
created_at: 2025-08-18T06:48:40Z
updated_at: 2025-08-18T18:25:30Z
url: https://github.com/astral-sh/ty/issues/1035
synced_at: 2026-01-10T02:06:24Z
```

# Inlay Hint Improvment

---

_Issue opened by @alirezanobakht13 on 2025-08-18 06:48_

Inlay hint feature is super useful in the area of active line, but when it is activated everywhere, it makes code kind of messy and in my opinion sometimes harder to read. It would be great if it can be displayed only near active line of coding, and not in other lines. similar idea from `Error Lens` extension: 

<img width="408" height="387" alt="Image" src="https://github.com/user-attachments/assets/ca399c6a-447b-419f-8419-fb6c904382b0" />

---

_Comment by @MichaReiser on 2025-08-18 06:58_

Thanks for the idea. 

I'm not sure this is something that's in ty's control. ty is based on the language server specification and uses the inlay hint request to provide the inlays https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_inlayHint and it doesn't contain any information about where in the code the user currently is. 

That means, only showing the active inlays is something that the client would have to do (in your case VS code?)

---

_Label `server` added by @MichaReiser on 2025-08-18 06:58_

---

_Label `wish` added by @MichaReiser on 2025-08-18 06:58_

---

_Comment by @dhruvmanila on 2025-08-18 07:11_

Yeah, this would part of the client implementation because the client knows where the cursor is and will be able to restrict the range in which to request inlay hints for.

---

_Comment by @alirezanobakht13 on 2025-08-18 07:22_

Ok then I will also ask them to add it.

---

_Closed by @carljm on 2025-08-18 18:25_

---
