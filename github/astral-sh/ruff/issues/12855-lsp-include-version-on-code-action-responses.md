---
number: 12855
title: "LSP: include `version` on code action responses"
type: issue
state: closed
author: LDAP
labels:
  - server
assignees: []
created_at: 2024-08-13T09:06:54Z
updated_at: 2024-08-13T14:32:37Z
url: https://github.com/astral-sh/ruff/issues/12855
synced_at: 2026-01-07T13:12:15-06:00
---

# LSP: include `version` on code action responses

---

_Issue opened by @LDAP on 2024-08-13 09:06_

Including `version` on code action responses would prevent outdated code actions being applied if the document has changed in the meantime.

See https://github.com/sublimelsp/LSP-ruff/issues/72

---

_Label `server` added by @MichaReiser on 2024-08-13 11:02_

---

_Comment by @dhruvmanila on 2024-08-13 14:02_

Can you clarify which "version" key are you talking about here? The `CodeAction` type doesn't have a `version` field: https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#codeAction

---

_Comment by @dhruvmanila on 2024-08-13 14:03_

Oh, do you mean the document version in `TextDocumentEdit`?

---

_Comment by @dhruvmanila on 2024-08-13 14:04_

I think this could be merged into https://github.com/astral-sh/ruff/issues/11730

---

_Comment by @LDAP on 2024-08-13 14:11_

@rchl

---

_Comment by @rchl on 2024-08-13 14:16_

Yes, document edits is what I meant to say in the other issue.

---

_Comment by @dhruvmanila on 2024-08-13 14:32_

Thanks for confirming. I'll merge this into #11730.

---

_Closed by @dhruvmanila on 2024-08-13 14:32_

---
