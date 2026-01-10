```yaml
number: 10782
title: "Rework linter API to remove a `Clone` implementation on `LinterSettings`"
type: issue
state: closed
author: snowsignal
labels:
  - server
assignees: []
created_at: 2024-04-04T22:59:34Z
updated_at: 2024-06-21T11:42:43Z
url: https://github.com/astral-sh/ruff/issues/10782
synced_at: 2026-01-10T11:09:53Z
```

# Rework linter API to remove a `Clone` implementation on `LinterSettings`

---

_Issue opened by @snowsignal on 2024-04-04 22:59_

In https://github.com/astral-sh/ruff/pull/10652, we had to implement `Clone` for `LinterSettings` and all sub-settings so that the `source.organizeImports` resolver could clone the settings from a reference and then modify `rules` to `[I001, I002]`. Since this cloning operation is wasteful (we only need to update one field), we should find a way to temporarily override values in `LinterSettings` without needing to clone the entire struct.

---

_Label `server` added by @snowsignal on 2024-04-04 22:59_

---

_Assigned to @snowsignal by @snowsignal on 2024-04-04 22:59_

---

_Added to milestone `Ruff Server: Beta` by @snowsignal on 2024-06-17 16:32_

---

_Removed from milestone `Ruff Server: Beta` by @snowsignal on 2024-06-17 16:32_

---

_Added to milestone `Ruff Server: Post-Stable` by @snowsignal on 2024-06-17 16:33_

---

_Comment by @MichaReiser on 2024-06-18 06:04_

I think we can ignore this for now. I expect that setting handling will be significantly different in Red knot. 

---

_Closed by @MichaReiser on 2024-06-21 11:42_

---
