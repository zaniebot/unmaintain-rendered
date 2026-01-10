```yaml
number: 243
title: "Use structured `Diagnostic`s instead of generic `anyhow::Result` in `run`"
type: issue
state: closed
author: MichaReiser
labels:
  - diagnostics
assignees: []
created_at: 2024-08-08T08:45:59Z
updated_at: 2025-05-07T16:00:29Z
url: https://github.com/astral-sh/ty/issues/243
synced_at: 2026-01-10T02:34:09Z
```

# Use structured `Diagnostic`s instead of generic `anyhow::Result` in `run`

---

_Issue opened by @MichaReiser on 2024-08-08 08:45_

The `run` function returns a `anyhow::Result<ExitStatus>`. The downside of this is that the `Error` is unstructured and unspecific about what went wrong. This limits the way we can present the error to the user. 

We should explore using a more structured `Error` similar to Biome's CLI Diagnostic (supports multiple code frames) or Miette (supports only a single code frame). 

https://github.com/MichaReiser/biome/blob/60671ec3a4c59421583d5d1ec8cfe30d45b27cd7/crates/biome_cli/src/diagnostics.rs#L18-L59

---

_Label `diagnostics` added by @MichaReiser on 2025-01-15 09:51_

---

_Comment by @MichaReiser on 2025-01-15 09:51_

@BurntSushi I just came across this somewhat older issue that's also related to diagnostics (less the rendering, more the "represent error-chains"/error type requirement of it)

---

_Assigned to @BurntSushi by @BurntSushi on 2025-01-15 16:27_

---

_Renamed from "[red-knot] Use structured `Diagnostic`s instead of generic `anyhow::Result` in `run`" to "Use structured `Diagnostic`s instead of generic `anyhow::Result` in `run`" by @MichaReiser on 2025-05-07 15:27_

---

_Comment by @MichaReiser on 2025-05-07 16:00_

I think this is out of scope for now.

---

_Closed by @MichaReiser on 2025-05-07 16:00_

---
