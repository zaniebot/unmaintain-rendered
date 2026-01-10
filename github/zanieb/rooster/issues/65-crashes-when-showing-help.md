---
number: 65
title: Crashes when showing help
type: issue
state: closed
author: MichaReiser
labels: []
assignees: []
created_at: 2025-05-13T09:44:45Z
updated_at: 2025-08-26T16:10:10Z
url: https://github.com/zanieb/rooster/issues/65
synced_at: 2026-01-10T00:06:37Z
---

# Crashes when showing help

---

_Issue opened by @MichaReiser on 2025-05-13 09:44_

```
✦ ❯ uvx --from rooster-blue rooster --help
Traceback (most recent call last):

  File "/Users/micha/.cache/uv/archive-v0/PdrOmYhOEVkySRZk3-Lix/bin/rooster", line 12, in <module>
    sys.exit(app())
             ~~~^^

TypeError: Parameter.make_metavar() missing 1 required positional argument: 'ctx'
````

---

_Closed by @zanieb on 2025-08-26 16:10_

---
