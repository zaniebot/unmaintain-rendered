```yaml
number: 1669
title: Signature output as markdown?
type: issue
state: open
author: klonuo
labels:
  - server
assignees: []
created_at: 2025-11-28T19:42:27Z
updated_at: 2025-11-28T19:50:31Z
url: https://github.com/astral-sh/ty/issues/1669
synced_at: 2026-01-10T01:58:59Z
```

# Signature output as markdown?

---

_Issue opened by @klonuo on 2025-11-28 19:42_

When I use autocomplete, signature is returned as plaintext:

<img width="912" height="463" alt="Image" src="https://github.com/user-attachments/assets/02f733b3-193f-41d7-9a24-3a51b56e61ec" />


while if I hover on the same symbol, it is returned in markdown:

<img width="630" height="536" alt="Image" src="https://github.com/user-attachments/assets/37fa6add-157b-4b5d-8092-d51dc007a9b8" />


As signature supports markdown for documentation parameter (https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#signatureInformation) I was wondering why is it not returned as markdown by `ty` server?

---

_Label `server` added by @Gankra on 2025-11-28 19:48_

---

_Added to milestone `Stable` by @Gankra on 2025-11-28 19:48_

---

_Comment by @Gankra on 2025-11-28 19:50_

If the protocol supports it then yeah we should probably be wrapping them in markdown fences. Although the issues described in https://github.com/astral-sh/ruff/pull/21438 are still a problem.

---
