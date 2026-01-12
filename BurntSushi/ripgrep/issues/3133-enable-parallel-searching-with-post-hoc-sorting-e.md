```yaml
number: 3133
title: "Enable parallel searching with post hoc sorting (e.g., `--sort=path`) for performance and deterministic output"
type: issue
state: closed
author: eval-exec
labels: []
assignees: []
created_at: 2025-08-24T08:58:00Z
updated_at: 2025-08-24T11:46:32Z
url: https://github.com/BurntSushi/ripgrep/issues/3133
synced_at: 2026-01-12T16:13:25Z
```

# Enable parallel searching with post hoc sorting (e.g., `--sort=path`) for performance and deterministic output

---

_@eval-exec_

#### Describe your feature request

`--sort=path` currently disables parallelism, forcing a slower, single-threaded search

A better approach would be to run searches in parallel, then buffer and sort the results by path before emitting—retaining both speed and determinism.

---

_Closed by @BurntSushi on 2025-08-24 11:45_

---

_Comment by @BurntSushi on 2025-08-24 11:46_

> [...] retaining both speed and determinism

... while ballooning memory usage.

---
