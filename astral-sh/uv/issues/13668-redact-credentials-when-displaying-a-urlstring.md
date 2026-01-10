```yaml
number: 13668
title: "Redact credentials when displaying a `UrlString`"
type: issue
state: open
author: jtfmumm
labels:
  - security
assignees: []
created_at: 2025-05-26T22:03:48Z
updated_at: 2025-11-07T15:33:16Z
url: https://github.com/astral-sh/uv/issues/13668
synced_at: 2026-01-10T03:23:54Z
```

# Redact credentials when displaying a `UrlString`

---

_Issue opened by @jtfmumm on 2025-05-26 22:03_

Recent work (#13560) added the `DisplaySafeUrl` type to make masking credentials the default behavior for displaying URLs. But `UrlString`s can be built directly from a `&str`, which makes no guarantees about whether they will contain credentials when displayed. Add a `Display` implementation that masks these by default.

---

_Label `security` added by @jtfmumm on 2025-05-26 22:03_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-06-26 07:11_

---

_Comment by @woodruffw on 2025-11-07 15:29_

Triage: #16623 describes another scenario where we don't redact credentials in URLs.

---

_Unassigned @jtfmumm by @konstin on 2025-11-07 15:33_

---
