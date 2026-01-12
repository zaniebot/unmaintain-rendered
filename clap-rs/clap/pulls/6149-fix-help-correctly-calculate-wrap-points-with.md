```yaml
number: 6149
title: "fix(help): Correctly calculate wrap points with ANSI escape codes "
type: pull_request
state: merged
author: epage
labels: []
assignees: []
merged: true
base: master
head: wrap
created_at: 2025-10-13T15:50:59Z
updated_at: 2025-10-13T16:10:55Z
url: https://github.com/clap-rs/clap/pull/6149
synced_at: 2026-01-12T16:14:18Z
```

# fix(help): Correctly calculate wrap points with ANSI escape codes 

---

_@epage_

We mixed up whether `i` represented the absolute position or our
incremental position.

Fixes #6147

---
