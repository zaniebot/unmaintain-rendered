---
number: 16966
title: "Red Knot playground panics when `knot.json` has a path specified"
type: issue
state: closed
author: InSyncWithFoo
labels:
  - playground
  - ty
assignees: []
created_at: 2025-03-25T12:29:52Z
updated_at: 2025-03-25T13:03:32Z
url: https://github.com/astral-sh/ruff/issues/16966
synced_at: 2026-01-07T13:12:16-06:00
---

# Red Knot playground panics when `knot.json` has a path specified

---

_Issue opened by @InSyncWithFoo on 2025-03-25 12:29_

Steps to reproduce:

1. Go to [the playground](https://playknot.ruff.rs).
2. Switch to `knot.json` and paste the following:

    ```json
    {
        "environment": {
            "extra-paths": [
                ""
            ]
        }
    }
    ```

An error message then appears, which doesn't go away even when the editor is blanked:

> Failed to update 'knot.json' options: recursive use of an object detected which would lead to unsafe aliasing in rust

Meanwhile, the browser console says:

> panicked at library/std/src/sys/pal/wasm/../unsupported/time.rs:31:9:
> time not implemented on this platform

> Error: recursive use of an object detected which would lead to unsafe aliasing in rust

> Failed to initialize playground. RuntimeError: unreachable


---

_Renamed from "Red Knot playground panics when `knot.json` has `environment.extra-paths` defined" to "Red Knot playground panics when `knot.json` has a path specified" by @InSyncWithFoo on 2025-03-25 12:30_

---

_Label `playground` added by @MichaReiser on 2025-03-25 12:40_

---

_Label `red-knot` added by @MichaReiser on 2025-03-25 12:40_

---

_Referenced in [astral-sh/ruff#16967](../../astral-sh/ruff/pulls/16967.md) on 2025-03-25 12:55_

---

_Closed by @MichaReiser on 2025-03-25 13:03_

---
