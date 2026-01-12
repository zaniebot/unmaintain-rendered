```yaml
number: 11595
title: "`ruff server` update `lsp-types` to 0.96.0"
type: issue
state: open
author: ismail
labels:
  - help wanted
  - server
assignees: []
created_at: 2024-05-29T07:05:32Z
updated_at: 2025-04-03T13:48:09Z
url: https://github.com/astral-sh/ruff/issues/11595
synced_at: 2026-01-12T15:54:51Z
```

# `ruff server` update `lsp-types` to 0.96.0

---

_@ismail_

Looks like https://github.com/gluon-lang/lsp-types has released version 0.96.0 with notebook support, would it be possible to use it instead of using a custom fork?

---

_Comment by @dhruvmanila on 2024-05-29 07:22_

Yeah, that should be possible although @snowsignal might know more if there's any other difference apart from the Notebook support but she's on PTO for the week. Feel free to open a PR if you're interested :)

---

_Label `help wanted` added by @dhruvmanila on 2024-05-29 07:22_

---

_Label `server` added by @dhruvmanila on 2024-05-29 07:22_

---

_Renamed from "Unfork lsp-types" to "`ruff server` unfork lsp-types" by @dhruvmanila on 2024-05-29 07:22_

---

_Renamed from "`ruff server` unfork lsp-types" to "`ruff server` update `lsp-types` to 0.96.0" by @dhruvmanila on 2024-05-29 07:23_

---

_Comment by @MichaReiser on 2024-05-29 12:04_

Yeah, I think that should be possible. All we needed was that the notebook PR gets merged and released and that's now the case. 

---

_Comment by @T-256 on 2024-05-31 12:47_

Also, we should consider https://github.com/gluon-lang/lsp-types/issues/284

---

_Comment by @MichaReiser on 2024-05-31 13:01_

Hmm, that sounds painful. I don't think we're in a rush updating. Let's see what they decide on.

---

_Comment by @T-256 on 2024-05-31 21:51_

cc @karthiknadig who announced `lsprotocol` crate [at here](https://github.com/astral-sh/ruff-lsp/issues/300#issuecomment-1813500582) and was looking for feedback. I recommend you try `ruff_server` migrated to `lsprotocol`. I tried but there were few blockers that stopped me on continue it. you can catch these blockers as feedback to improve `lsprotocol` crate.

---

_Comment by @snowsignal on 2024-06-03 20:05_

The issue blocking an upgrade to 0.96.0, as @T-256 pointed out, is https://github.com/gluon-lang/lsp-types/issues/284. We use the old `Url` type frequently in the codebase and the new `Uri` type lacks several key functions that we need, especially related to file path conversion.

I would be interested in exploring a move to `lsprotocol`, though as @T-256 said there could be blockers there too.

---

_Comment by @dhruvmanila on 2024-06-04 06:13_

> I tried but there were few blockers that stopped me on continue it.

Can you enumerate the blockers here? We'd appreciate that. It could also be useful to provide the diff here and anyone from the team can enumerate the said blockers.

---

_Added to milestone `Ruff Server: Post-Stable` by @snowsignal on 2024-06-17 16:34_

---

_Removed from milestone `Ruff Server: Post-Stable` by @dhruvmanila on 2025-04-03 13:48_

---
