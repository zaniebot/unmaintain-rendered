```yaml
number: 7665
title: "Don't show deploy notification when we don't need to"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/dont-deploy
created_at: 2024-09-24T17:33:06Z
updated_at: 2024-09-24T17:42:15Z
url: https://github.com/astral-sh/uv/pull/7665
synced_at: 2026-01-12T16:07:56Z
```

# Don't show deploy notification when we don't need to

---

_@konstin_

To scope permissions more tightly, we run the upload test in an environment. This makes GitHub show `{user} deployed to uv-test-publish` messages, even though we skipped the actual publish step. We now guard the whole action, so the pseudo-deploy is only shown when the publishing code changed and we run that trusted publishing test.


---

_Label `internal` added by @konstin on 2024-09-24 17:33_

---

_Review requested from @charliermarsh by @konstin on 2024-09-24 17:33_

---

_@charliermarsh approved on 2024-09-24 17:33_

Thanks

---

_Merged by @konstin on 2024-09-24 17:42_

---

_Closed by @konstin on 2024-09-24 17:42_

---

_Branch deleted on 2024-09-24 17:42_

---
