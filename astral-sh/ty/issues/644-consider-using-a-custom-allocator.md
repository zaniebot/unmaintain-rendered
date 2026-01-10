```yaml
number: 644
title: Consider using a custom allocator
type: issue
state: open
author: MichaReiser
labels:
  - memory
assignees: []
created_at: 2025-06-13T06:39:48Z
updated_at: 2025-12-31T15:27:30Z
url: https://github.com/astral-sh/ty/issues/644
synced_at: 2026-01-10T01:56:40Z
```

# Consider using a custom allocator

---

_Issue opened by @MichaReiser on 2025-06-13 06:39_

Mainly, mi-malloc. Considerations here are

* peak memory consumption
* long-running memory consumption (fragmentation)
* performance

CC: @ibraheemdev 

---

_Label `memory` added by @MichaReiser on 2025-06-13 06:39_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 15:02_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---

_Comment by @MichaReiser on 2025-12-31 15:27_

We now use jemalloc on linux

---
