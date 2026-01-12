```yaml
number: 324
title: "Progress bar shows `0/0` at initialization"
type: issue
state: closed
author: charliermarsh
labels:
  - cli
assignees: []
created_at: 2025-05-12T03:02:34Z
updated_at: 2025-05-12T19:07:56Z
url: https://github.com/astral-sh/ty/issues/324
synced_at: 2026-01-12T15:54:22Z
```

# Progress bar shows `0/0` at initialization

---

_@charliermarsh_

If you have a large project, then it shows 0/0 for a while (presumedly, while we count the files). We should either avoid showing a count at all until we know the number of files, or avoid showing the progress bar at all until we know the number of files.

(There are some progress bars in uv, if a reference is helpful.)

---

_Comment by @MichaReiser on 2025-05-12 06:27_

CC: @ibraheemdev 

---

_Label `cli` added by @MichaReiser on 2025-05-12 06:27_

---

_Closed by @ibraheemdev on 2025-05-12 19:07_

---
