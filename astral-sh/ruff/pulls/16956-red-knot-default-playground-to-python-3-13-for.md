```yaml
number: 16956
title: "[red-knot] Default playground to Python 3.13 for real"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/playground-313
created_at: 2025-03-24T16:35:42Z
updated_at: 2025-03-24T16:40:51Z
url: https://github.com/astral-sh/ruff/pull/16956
synced_at: 2026-01-12T15:55:59Z
```

# [red-knot] Default playground to Python 3.13 for real

---

_@MichaReiser_

## Summary

Default to 3.13 for good. 

I incorrectly used `workspace.updateOptions` instead of `updateOptions` where the latter has a fallback.

## Test Plan

```py
import os

import sys

reveal_type(sys.version_info.minor)
```

reveals 13 on initial page load


---

_Label `playground` added by @MichaReiser on 2025-03-24 16:35_

---

_Label `red-knot` added by @MichaReiser on 2025-03-24 16:35_

---

_Merged by @MichaReiser on 2025-03-24 16:40_

---

_Closed by @MichaReiser on 2025-03-24 16:40_

---

_Branch deleted on 2025-03-24 16:40_

---
