```yaml
number: 352
title: Update rooster
type: pull_request
state: merged
author: MichaReiser
labels:
  - release
assignees: []
merged: true
base: main
head: micha/update-rooster
created_at: 2025-05-13T10:11:25Z
updated_at: 2025-05-13T10:19:37Z
url: https://github.com/astral-sh/ty/pull/352
synced_at: 2026-01-12T15:54:27Z
```

# Update rooster

---

_@MichaReiser_

## Summary

Updates rooster to fix an invalid attribute access when using `--version` (which we need to use to bump to `0.0.1.alpha.1`

https://github.com/zanieb/rooster/pull/67

## Test Plan

Running the release script with this commit no longer crashes


---

_Label `release` added by @MichaReiser on 2025-05-13 10:11_

---

_Review requested from @zanieb by @MichaReiser on 2025-05-13 10:11_

---

_Merged by @MichaReiser on 2025-05-13 10:19_

---

_Closed by @MichaReiser on 2025-05-13 10:19_

---

_Branch deleted on 2025-05-13 10:19_

---
