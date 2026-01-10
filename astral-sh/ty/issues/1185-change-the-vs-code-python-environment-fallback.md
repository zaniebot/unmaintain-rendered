---
number: 1185
title: Change the VS code python environment fallback behavior
type: issue
state: closed
author: MichaReiser
labels:
  - configuration
  - server
assignees: []
created_at: 2025-09-15T06:04:38Z
updated_at: 2025-12-31T15:24:54Z
url: https://github.com/astral-sh/ty/issues/1185
synced_at: 2026-01-10T01:51:14Z
---

# Change the VS code python environment fallback behavior

---

_Issue opened by @MichaReiser on 2025-09-15 06:04_

I've to double check if this is still the case after my PTO but I'm opening this issue so that I don't forget about it. 

The behavior that I implemented a few months ago is that the Python environment in VS code takes precedence over ty's other Python environment detection. For example, the selected Python environment takes precedence over a local `.venv` folder or an active virtual environment environment variable. 

I've run into situations where I found this behavior confusing because the CLI picks up the imports just fine, but all imports fail in VS Code. This has also confused other users, e.g. see this [Discord thread](https://discord.com/channels/1039017663004942429/1408861479939211385).

I think we should change the precedence to only use the VS code selected interpreter last. This ensures that the CLI and the Python extension agree on the same Python environment. 

The only downside that I can see is that this can break existing setups where a project uses a `.venv` folder but wants to use a different virtual environment. If this is a common use case, we might then need to introduce an editor setting to configure the precedence.

---

_Added to milestone `Beta` by @MichaReiser on 2025-09-15 06:04_

---

_Label `configuration` added by @MichaReiser on 2025-09-15 06:04_

---

_Label `server` added by @MichaReiser on 2025-09-15 06:04_

---

_Renamed from "Consider changing the VS code python environment fallback behavior" to "Change the VS code python environment fallback behavior" by @MichaReiser on 2025-09-19 08:29_

---

_Comment by @MichaReiser on 2025-09-19 08:33_

Zed's extension uses this field to pass the python environment

---

_Assigned to @MichaReiser by @MichaReiser on 2025-09-19 08:36_

---

_Comment by @MichaReiser on 2025-09-22 17:43_

From my conversation with Zed (they use `pythonExtension` and a change to its precedence might be a breaking change for them):

> but could your issue be solved by comparing Ty's detected venv against the one from extension and - if there's a mismatch - showing a pop-up to let users decide what venv Ty should use? It sounds a bit like your users are confused by a mismatch in behaviour between the CLI and a language server, but at the same time they did not check VSC's active python interpreter (edited)

Implementing a picker might be non-trivial but logging a warning or showing a warning message would be a good starting point.

---

_Removed from milestone `Beta` by @MichaReiser on 2025-11-10 10:01_

---

_Closed by @MichaReiser on 2025-12-31 15:24_

---
