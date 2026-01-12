```yaml
number: 7820
title: "chore(renovate): enable `regex` manager"
type: pull_request
state: merged
author: mkniewallner
labels:
  - internal
assignees: []
merged: true
base: main
head: chore/enable-renovate-regex-manager
created_at: 2024-09-30T21:17:05Z
updated_at: 2024-09-30T21:33:05Z
url: https://github.com/astral-sh/uv/pull/7820
synced_at: 2026-01-12T16:08:01Z
```

# chore(renovate): enable `regex` manager

---

_@mkniewallner_

## Summary

Investigated https://github.com/astral-sh/uv/pull/7807#issuecomment-2384080360, and the reason why the PR mentioned in the comment did not work in the end is because we only opt-in for specific managers in Renovate configuration. By enabling [regex](https://docs.renovatebot.com/modules/manager/regex/) manager, we should now get proper updates to documentation references.

## Test Plan

Tested enabling specific managers (including `regex` one) in https://github.com/mkniewallner/mkv-playground/pull/18, and Renovate was still able to detect regex dependencies.

---

_@charliermarsh approved on 2024-09-30 21:28_

Interesting, thanks. Let's give it a shot!

---

_Merged by @charliermarsh on 2024-09-30 21:28_

---

_Closed by @charliermarsh on 2024-09-30 21:28_

---

_Label `internal` added by @charliermarsh on 2024-09-30 21:28_

---

_Comment by @mkniewallner on 2024-09-30 21:32_

Looks better in https://github.com/astral-sh/uv/issues/2658:
<img width="280" alt="Screenshot 2024-09-30 at 23 29 56" src="https://github.com/user-attachments/assets/98fbb684-52dd-4141-aec6-6a526f26cb50">

"Unfortunately" everything is already using the latest major version, so we won't see any PR from Renovate before some time 😄 

---

_Branch deleted on 2024-09-30 21:33_

---
