```yaml
number: 542
title: "Convert tracing warnings during `site-packages` discovery to diagnostic warnings"
type: issue
state: open
author: AlexWaygood
labels:
  - configuration
  - diagnostics
  - imports
assignees: []
created_at: 2025-05-29T20:09:04Z
updated_at: 2025-11-13T06:55:19Z
url: https://github.com/astral-sh/ty/issues/542
synced_at: 2026-01-10T02:06:24Z
```

# Convert tracing warnings during `site-packages` discovery to diagnostic warnings

---

_Issue opened by @AlexWaygood on 2025-05-29 20:09_

This file has lots of places where it emits tracing warnings if `site-packages` discovery doesn't go to plan: https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/site_packages.rs. Ideally we should convert these to warning-level diagnostics so that they're more visible to IDE users (suggested by @MichaReiser in https://github.com/astral-sh/ruff/pull/18335#discussion_r2111051215)

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-29 20:09_

---

_Label `imports` added by @AlexWaygood on 2025-05-29 20:09_

---

_Comment by @MichaReiser on 2025-05-29 20:21_

To clarify: we should only convert the once that require user interaction (where we expect them to change something) because warning diagnostics can change the exit code

---

_Comment by @AlexWaygood on 2025-05-29 20:25_

Hrm. A message like this I think would be useful for the user to see (it'll help them diagnose the cause of type-checking diagnostics) but I don't know what we expect the user to do about it. It most likely indicates a bug in uv or ty, if it happens! But I don't think we should fail the process, because type-checking _might_ proceed just fine.

What about an info-level diagnostic? Would we still exit with code 0 if we did that?

---

_Comment by @AlexWaygood on 2025-05-29 20:27_

I guess one action the user could take in response to that diagnostic is to not use `uv run --with=ty`, and instead run ty using some other uv invocation.

---

_Label `configuration` added by @carljm on 2025-11-13 06:55_

---
