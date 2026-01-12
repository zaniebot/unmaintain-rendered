```yaml
number: 14419
title: Reduce index credential stashing code duplication
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/reduce-index-credential-duplication
created_at: 2025-07-02T13:08:10Z
updated_at: 2025-07-02T13:25:58Z
url: https://github.com/astral-sh/uv/pull/14419
synced_at: 2026-01-12T16:11:12Z
```

# Reduce index credential stashing code duplication

---

_@konstin_

Reduces some duplicate code around index credentials.


---

_Label `internal` added by @konstin on 2025-07-02 13:08_

---

_Comment by @konstin on 2025-07-02 13:08_

I wonder whether we can avoid having to call this manually, e.g. by moving it into the registry client builder? I think missing this call in uv publish in causing https://github.com/astral-sh/uv/issues/11836, and it seems easy to miss such a call.

---

_Comment by @zanieb on 2025-07-02 13:20_

I'd be supportive of exploring that, it's definitely known this is brittle. This seems like a nice improvement,.

---

_@zanieb approved on 2025-07-02 13:20_

---

_Renamed from "Reduce index credential duplication" to "Reduce index credential stashing code duplication" by @zanieb on 2025-07-02 13:20_

---

_Merged by @konstin on 2025-07-02 13:25_

---

_Closed by @konstin on 2025-07-02 13:25_

---

_Branch deleted on 2025-07-02 13:25_

---
