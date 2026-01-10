```yaml
number: 1036
title: Return documentation with CompletionItem
type: issue
state: closed
author: JaRoSchm
labels:
  - server
assignees: []
created_at: 2025-08-18T07:23:22Z
updated_at: 2025-08-21T06:35:11Z
url: https://github.com/astral-sh/ty/issues/1036
synced_at: 2026-01-10T02:06:24Z
```

# Return documentation with CompletionItem

---

_Issue opened by @JaRoSchm on 2025-08-18 07:23_

Hi, many editor allow to show the signature and docstring next to each suggested completion item. See for example this screenshot where python-lsp-server and neovim are used:

<img width="1003" height="434" alt="Image" src="https://github.com/user-attachments/assets/d12c7fac-ebd1-4635-bd5f-e52e0a807d44" />

If ty is used, no documentation is shown:

<img width="474" height="142" alt="Image" src="https://github.com/user-attachments/assets/e5461e33-d0ad-4355-b795-41e7c114445c" />

I suppose that for this `documentation` has to be returned with each `CompletionItem`, see https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#completionItem. It would be nice if this was also supported by ty.

---

_Comment by @MichaReiser on 2025-08-18 07:46_

Thanks for opening this issue.

@Gankra I think this should be fairly easy to add with all the machinery you added for hover/goto definition et al

---

_Label `server` added by @MichaReiser on 2025-08-18 07:46_

---

_Added to milestone `Beta` by @MichaReiser on 2025-08-18 07:46_

---

_Assigned to @Gankra by @Gankra on 2025-08-18 19:16_

---

_Closed by @Gankra on 2025-08-20 21:00_

---

_Comment by @JaRoSchm on 2025-08-21 06:35_

Thank you!

---
