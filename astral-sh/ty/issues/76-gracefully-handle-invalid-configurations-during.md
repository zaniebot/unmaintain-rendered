```yaml
number: 76
title: Gracefully handle invalid configurations during startup
type: issue
state: closed
author: MichaReiser
labels:
  - configuration
  - server
  - fatal
assignees: []
created_at: 2025-03-28T18:14:27Z
updated_at: 2025-07-14T10:27:53Z
url: https://github.com/astral-sh/ty/issues/76
synced_at: 2026-01-12T15:54:22Z
```

# Gracefully handle invalid configurations during startup

---

_@MichaReiser_

* Create an invalid ty configuration (e.g. a syntax error)
* Start VS Code
* ty crashes

---

_Label `server` added by @MichaReiser on 2025-03-28 18:14_

---

_Renamed from "[red-knot] LSP crashes when the configuration is invalid" to "LSP crashes when the configuration is invalid" by @MichaReiser on 2025-05-07 15:13_

---

_Added to milestone `beta` by @MichaReiser on 2025-05-07 15:59_

---

_Label `configuration` added by @AlexWaygood on 2025-05-11 07:56_

---

_Label `fatal` added by @AlexWaygood on 2025-05-11 07:56_

---

_Renamed from "LSP crashes when the configuration is invalid" to "Gracefully handle invalid configurations during startup" by @MichaReiser on 2025-05-28 11:18_

---

_Assigned to @MichaReiser by @dhruvmanila on 2025-07-01 03:03_

---

_Comment by @T-256 on 2025-07-11 21:16_

As VSCode user who uses rust-analyzer, here is the fallback scenario I would expect when failed to load configuration:
The prerequires here is ty-vscode could improve by adding a status button in the status bar and snapshot already loaded configurations.
When there is invalid configuration, status button should be change to warning and show what is it currently fallback to.
When there was snapshotted configuration, it goes with that and still provide an option to user to reset to default configuration when hovering over status button. (bonus here would be an option to open snapshotted configuration in new tab)

P.S I didn't have chance to test ty-vscode yet, so apologies if some of them already implemented. 

---

_Closed by @MichaReiser on 2025-07-14 10:27_

---
