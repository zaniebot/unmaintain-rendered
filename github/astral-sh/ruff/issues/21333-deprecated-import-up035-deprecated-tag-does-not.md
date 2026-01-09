---
number: 21333
title: "`deprecated-import` (`UP035`) - `Deprecated` tag does not appear on deprecated types that are replaced with a builtin type"
type: issue
state: closed
author: DetachHead
labels:
  - bug
  - good first issue
assignees: []
created_at: 2025-11-08T03:35:31Z
updated_at: 2025-11-12T07:17:30Z
url: https://github.com/astral-sh/ruff/issues/21333
synced_at: 2026-01-07T13:12:16-06:00
---

# `deprecated-import` (`UP035`) - `Deprecated` tag does not appear on deprecated types that are replaced with a builtin type

---

_Issue opened by @DetachHead on 2025-11-08 03:35_

### Summary

```py
from typing import List  # no tag
from typing import Set  # no tag
from typing import Sequence  # tag
from typing import Generator  # tag
```
<img width="251" height="88" alt="Image" src="https://github.com/user-attachments/assets/c23cb5eb-bd40-49f8-82c0-14a095096dbc" />

### Version

0.14.4

---

_Comment by @MichaReiser on 2025-11-08 17:23_

I'm not sure I understand what this issue is about. Can you say a bit more?

---

_Comment by @DetachHead on 2025-11-09 06:04_

the strikethrough on the `Sequence` and `Generator` imports. it's a called a [diagnostic tag](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#diagnosticTag) in the language server spec.

this issue is just pointing out that for some reason only some deprecated imports get the tag

---

_Comment by @MichaReiser on 2025-11-10 09:04_

Oh I see. I think the bug is that we aren't adding the tag for:

https://github.com/astral-sh/ruff/blob/a630a46849bd25c461aeda2e1efa73c0e1b37a34/crates/ruff_linter/src/rules/pyupgrade/rules/deprecated_import.rs#L768-L775

---

_Label `bug` added by @MichaReiser on 2025-11-10 09:04_

---

_Label `good first issue` added by @MichaReiser on 2025-11-10 09:04_

---

_Referenced in [astral-sh/ruff#21396](../../astral-sh/ruff/pulls/21396.md) on 2025-11-12 05:58_

---

_Comment by @11happy on 2025-11-12 05:59_

was an easy fix ! 
linked the PR : )
Thank you

---

_Closed by @MichaReiser on 2025-11-12 07:17_

---
