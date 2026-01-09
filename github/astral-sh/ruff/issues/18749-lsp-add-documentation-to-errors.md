---
number: 18749
title: "lsp: add documentation to errors"
type: issue
state: closed
author: KotlinIsland
labels:
  - wish
  - server
assignees: []
created_at: 2025-06-18T13:08:59Z
updated_at: 2025-07-11T15:37:30Z
url: https://github.com/astral-sh/ruff/issues/18749
synced_at: 2026-01-07T13:12:16-06:00
---

# lsp: add documentation to errors

---

_Issue opened by @KotlinIsland on 2025-06-18 13:08_

### Summary

currently, the error messages for a report don't show, or link to the documentation at all, and can end up being extremely mysterious:
<img width="500" alt="Image" src="https://github.com/user-attachments/assets/24b5973d-df0c-4536-82a6-8949b48fe5a6" />

epic error doc for ignore code:
<img width="684" alt="Image" src="https://github.com/user-attachments/assets/5cd43726-6ed6-4f3d-8b8d-2b76f780cd02" />

### Version

_No response_

---

_Comment by @MichaReiser on 2025-06-18 13:11_

Related https://github.com/astral-sh/ruff-vscode/issues/723

---

_Label `wish` added by @MichaReiser on 2025-06-18 13:11_

---

_Label `server` added by @MichaReiser on 2025-06-18 13:11_

---

_Comment by @dhruvmanila on 2025-06-19 06:37_

Do you mean something like this? Here, the `T201` at the end of the diagnostic message is a link to the documentation page of `T201`. This is sent in the diagnostic payload to the client.

<img width="560" alt="Image" src="https://github.com/user-attachments/assets/7ffbbbb7-8266-4f31-84f6-ad792458a433" />

---

_Comment by @KotlinIsland on 2025-06-19 07:49_

hmm, yes, that does seem good. is there a reason we can't show the same as hovering a `noqa` code?

---

_Comment by @dhruvmanila on 2025-06-19 09:50_

> is there a reason we can't show the same as hovering a `noqa` code?

Are you suggesting to display the link to the documentation in the hover of `noqa` code instead or in addition to the documentation itself?

---

_Comment by @KotlinIsland on 2025-07-11 04:31_

i'm suggesting to include the documentation that is included in the `noqa` hover to be included in the hover for the diagnostic, instead of just the link to the documentation

---

_Comment by @MichaReiser on 2025-07-11 08:33_

> i'm suggesting to include the documentation that is included in the noqa hover to be included in the hover for the diagnostic, instead of just the link to the documentation

I don't think we should do that. It would make the diagnostic very verbose and users will strugle to see what the error is. Especially because the rule documentation uses headers and such that will be more prominent than the rror itself. It's very important to keep diagnostics concise. 

---

_Closed by @MichaReiser on 2025-07-11 08:33_

---

_Comment by @KotlinIsland on 2025-07-11 15:23_

i'm not suggeting that the documentation be included in the diagnostic, but in the hover

---

_Comment by @MichaReiser on 2025-07-11 15:37_

Yeah, I understand that. I still think it's distracting from what's most important, the diagnostic message (in the hover) if you end up with this large blob of documentation next it

---
