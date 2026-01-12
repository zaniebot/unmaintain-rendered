```yaml
number: 16759
title: " Don't check file URLs for ambiguously parsed URLs"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/dont-reject-file-urls
created_at: 2025-11-17T12:38:53Z
updated_at: 2025-11-17T14:16:15Z
url: https://github.com/astral-sh/uv/pull/16759
synced_at: 2026-01-12T16:12:25Z
```

#  Don't check file URLs for ambiguously parsed URLs

---

_@konstin_

Fixes https://github.com/astral-sh/uv/issues/16756
Follow-up for https://github.com/astral-sh/uv/pull/16622

I noticed that rustfmt couldn't handle the check, so I moved the code around in the first two commits.

---

_Review requested from @woodruffw by @konstin on 2025-11-17 12:38_

---

_Label `bug` added by @konstin on 2025-11-17 12:38_

---

_@woodruffw approved on 2025-11-17 12:44_

Thanks @konstin!

---

_Review comment by @woodruffw on `crates/uv-redacted/src/lib.rs`:82 on 2025-11-17 12:46_

Nit, not a blocker: might be worth updating the comment to explain that we don't apply this check to file URLs, since they don't have credentials/it'll easily snare on Windows paths. But I think that's also clear in the body below so not a big deal either way 🙂

---

_@woodruffw reviewed on 2025-11-17 12:46_

---

_@konstin reviewed on 2025-11-17 14:00_

---

_Review comment by @konstin on `crates/uv-redacted/src/lib.rs`:82 on 2025-11-17 14:00_

I added a comment inline.

---

_Merged by @konstin on 2025-11-17 14:16_

---

_Closed by @konstin on 2025-11-17 14:16_

---

_Branch deleted on 2025-11-17 14:16_

---
