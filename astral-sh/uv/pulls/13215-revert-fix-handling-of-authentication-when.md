```yaml
number: 13215
title: Revert fix handling of authentication when encountering redirects
type: pull_request
state: merged
author: jtfmumm
labels:
  - bug
assignees: []
merged: true
base: main
head: jtfm/revert-redirect-handling
created_at: 2025-04-30T08:14:15Z
updated_at: 2025-04-30T09:12:09Z
url: https://github.com/astral-sh/uv/pull/13215
synced_at: 2026-01-10T11:10:41Z
```

# Revert fix handling of authentication when encountering redirects

---

_Pull request opened by @jtfmumm on 2025-04-30 08:14_

These changes to redirect handling appear to have caused #13208. This PR reverts the redirect changes to give us time to investigate.


---

_Closed by @jtfmumm on 2025-04-30 08:21_

---

_Reopened by @jtfmumm on 2025-04-30 08:21_

---

_Review requested from @konstin by @jtfmumm on 2025-04-30 08:30_

---

_Comment by @jenshnielsen on 2025-04-30 08:36_

@jtfmumm I can confirm that `cargo run pip install somepackage` with uv build from this pr works whereas `uv pip install somepackage` fails (for uv 0.7.0) 

---

_@konstin approved on 2025-04-30 08:52_

---

_Merged by @jtfmumm on 2025-04-30 08:53_

---

_Closed by @jtfmumm on 2025-04-30 08:53_

---

_Branch deleted on 2025-04-30 08:53_

---

_Label `bug` added by @konstin on 2025-04-30 09:12_

---
