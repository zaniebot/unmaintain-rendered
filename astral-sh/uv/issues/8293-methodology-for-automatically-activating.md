```yaml
number: 8293
title: Methodology for Automatically Activating/Deactivating venvs When Changing Directories in a “uv-owned” Project
type: issue
state: open
author: metersk
labels: []
assignees: []
created_at: 2024-10-17T15:47:03Z
updated_at: 2024-10-17T16:12:47Z
url: https://github.com/astral-sh/uv/issues/8293
synced_at: 2026-01-12T15:59:23Z
```

# Methodology for Automatically Activating/Deactivating venvs When Changing Directories in a “uv-owned” Project

---

_@metersk_

I often forget to name my virtual environments, so my prompt always displays `.venv ❯` when I’m in a `uv` project. Occasionally, I’ll `cd` into another `uv` project without realizing that the previous virtual environment is still active.

Does anyone have a method for automatically activating the virtual environment of the current directory and deactivating it when I change to a different directory?

---

_Renamed from "Methodology for automatically activating/deactivating venvs when you `cd` in or out of a "uv-owned" dir?" to "Methodology for Automatically Activating/Deactivating venvs When Changing Directories in a “uv-owned” Project" by @metersk on 2024-10-17 15:48_

---

_Comment by @dschneiderch on 2024-10-17 16:12_

check out https://direnv.net/
there is a community-contributed hook for uv too.

---
