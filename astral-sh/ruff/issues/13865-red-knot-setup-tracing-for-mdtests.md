```yaml
number: 13865
title: "[red-knot] Setup tracing for mdtests"
type: issue
state: closed
author: MichaReiser
labels:
  - help wanted
  - testing
  - ty
assignees: []
created_at: 2024-10-21T19:18:24Z
updated_at: 2024-12-13T09:10:03Z
url: https://github.com/astral-sh/ruff/issues/13865
synced_at: 2026-01-10T11:09:55Z
```

# [red-knot] Setup tracing for mdtests

---

_Issue opened by @MichaReiser on 2024-10-21 19:18_

We should allow enabling tracing for mdtests. 

One option is to reuse the same pattern as for the CLI:
* By default, tracing defaults to WARN
* setting the env variable `RED_KNOT_LOG` allows customizing the tracing level. 

We can't use `setup_tracing` because tracing can only be initialized once (globally) but we have to initialize tracing on a per test level. We should look into if we can reuse some of the logic from `setup_logging_with_filter`







---

_Label `help wanted` added by @MichaReiser on 2024-10-21 19:18_

---

_Label `red-knot` added by @MichaReiser on 2024-10-21 19:18_

---

_Label `testing` added by @AlexWaygood on 2024-10-21 21:52_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-12-12 12:58_

---

_Closed by @MichaReiser on 2024-12-13 09:10_

---

_Closed by @MichaReiser on 2024-12-13 09:10_

---
