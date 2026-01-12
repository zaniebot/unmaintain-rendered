```yaml
number: 14229
title: Is removing a preview tag in uv versioning considered a breaking change?
type: issue
state: closed
author: FishAlchemist
labels:
  - question
assignees: []
created_at: 2025-06-24T05:35:54Z
updated_at: 2025-06-24T08:52:38Z
url: https://github.com/astral-sh/uv/issues/14229
synced_at: 2026-01-12T16:01:45Z
```

# Is removing a preview tag in uv versioning considered a breaking change?

---

_@FishAlchemist_

### Question

If so, should the preview tag only be removed when a major version release is confirmed? 
* https://github.com/astral-sh/uv/pull/14119

The PR mentioned above actually seems to be included in version ``0.7.14``.

![Image](https://github.com/user-attachments/assets/3398d4ca-94e8-4203-90ea-c7e1b9f0e1e8)

This is the version control document for UV.
https://docs.astral.sh/uv/reference/policies/versioning/



### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @FishAlchemist on 2025-06-24 05:35_

---

_Comment by @konstin on 2025-06-24 08:39_

Stabilizing a previously unstable feature is adding a feature: No previously working stable workflow breaks wit this change, so we don't consider it a breaking change. Additionally, you can still pass `--preview` to a stable feature and it will just be ignored.

---

_Comment by @FishAlchemist on 2025-06-24 08:52_

I see, thank you.

---

_Closed by @FishAlchemist on 2025-06-24 08:52_

---
