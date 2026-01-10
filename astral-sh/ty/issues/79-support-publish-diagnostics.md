```yaml
number: 79
title: Support publish diagnostics
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-03-14T10:37:46Z
updated_at: 2025-05-28T07:45:12Z
url: https://github.com/astral-sh/ty/issues/79
synced_at: 2026-01-10T02:34:09Z
```

# Support publish diagnostics

---

_Issue opened by @MichaReiser on 2025-03-14 10:37_

Support publish diagnostics for clients not supporting pull diagnostics

---

_Label `server` added by @MichaReiser on 2025-03-14 10:37_

---

_Comment by @MichaReiser on 2025-03-14 10:38_


@dhruvmanila could you add some more details to the summary on why it's important to use publish diagnostics? Do we need this for project-level diagnostics with external changes? Will this become the default?

---

_Comment by @InSyncWithFoo on 2025-03-27 05:45_

I <em>think</em> I want this. Neither IntelliJ's native client nor LSP4IJ supports `textDocument/diagnostics`; I'm making do with a homebrew implementation but I would prefer it if I didn't have to.

---

_Comment by @angelozerr on 2025-04-11 17:37_

Lsp4ij supports now pull diagnostics, so it should work.

---

_Comment by @InSyncWithFoo on 2025-04-17 22:34_

JetBrains [just added support](https://youtrack.jetbrains.com/issue/IJPL-172853) for pull as well, slated to be released as part of 2025.1.1 (sometime in May).

---

_Renamed from "[red-knot] Use publish diagnostics" to "Use publish diagnostics" by @MichaReiser on 2025-05-07 15:13_

---

_Renamed from "Use publish diagnostics" to "Support publish diagnostics" by @MichaReiser on 2025-05-19 07:11_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-05-21 14:51_

---

_Closed by @dhruvmanila on 2025-05-28 07:45_

---

_Closed by @dhruvmanila on 2025-05-28 07:45_

---
