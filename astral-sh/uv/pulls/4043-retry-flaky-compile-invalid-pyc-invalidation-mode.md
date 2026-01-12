```yaml
number: 4043
title: "Retry flaky `compile_invalid_pyc_invalidation_mode` test "
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/test-flake-hack
created_at: 2024-06-05T11:17:47Z
updated_at: 2024-06-05T18:37:18Z
url: https://github.com/astral-sh/uv/pull/4043
synced_at: 2026-01-12T16:06:00Z
```

# Retry flaky `compile_invalid_pyc_invalidation_mode` test 

---

_@konstin_

Retry the flaky `compile_invalid_pyc_invalidation_mode` if it fails. I don't understand why this happening in the first place (we have code that should catch those cases, but also those cases shouldn't be happening at all) and this is terrible hack, but it fixes the test flakes.

Fixes #2672

---

_Label `internal` added by @konstin on 2024-06-05 11:17_

---

_Comment by @konstin on 2024-06-05 18:37_

Going to merge this cause this test is now almost constantly failing for me.

---

_Merged by @konstin on 2024-06-05 18:37_

---

_Closed by @konstin on 2024-06-05 18:37_

---

_Branch deleted on 2024-06-05 18:37_

---
