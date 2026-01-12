```yaml
number: 19438
title: "[ty] Use vec and build `HashMap` from vec"
type: pull_request
state: closed
author: MichaReiser
labels: []
assignees: []
draft: true
base: main
head: micha/use-vec-semantic-index
created_at: 2025-07-20T15:09:20Z
updated_at: 2025-07-20T15:45:10Z
url: https://github.com/astral-sh/ruff/pull/19438
synced_at: 2026-01-12T15:56:39Z
```

# [ty] Use vec and build `HashMap` from vec

---

_@MichaReiser_

_No description provided._

---

_Comment by @github-actions[bot] on 2025-07-20 15:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @MichaReiser on 2025-07-20 15:45_

Interesting. The idea here was that resizing a hash map is more expensive than resizing a vec (because it requires rehashing). But it seems that the perf impact is neglectible. 

---

_Closed by @MichaReiser on 2025-07-20 15:45_

---
