```yaml
number: 17486
title: Normalize test snapshots
type: pull_request
state: open
author: konstin
labels:
  - internal
assignees: []
base: main
head: konsti/insta-regenerate
created_at: 2026-01-15T15:33:34Z
updated_at: 2026-01-15T15:33:34Z
url: https://github.com/astral-sh/uv/pull/17486
synced_at: 2026-01-15T15:50:34Z
```

# Normalize test snapshots

---

_@konstin_

This reduces the churn when changing test snapshots, as insta updates the quotes when the contents change. Instead, we decouple it by updating the quotes in bulk here.

Created by:

```
cargo insta test --accept --force-update-snapshots
```


---

_Label `internal` added by @konstin on 2026-01-15 15:33_

---
