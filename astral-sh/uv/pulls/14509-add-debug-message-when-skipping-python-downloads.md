```yaml
number: 14509
title: Add debug message when skipping Python downloads
type: pull_request
state: merged
author: Grinkers
labels:
  - error messages
  - tracing
assignees: []
merged: true
base: main
head: debug-message-on-skip-download
created_at: 2025-07-08T18:56:52Z
updated_at: 2025-07-09T16:04:30Z
url: https://github.com/astral-sh/uv/pull/14509
synced_at: 2026-01-12T16:11:15Z
```

# Add debug message when skipping Python downloads

---

_@Grinkers_

# Description
Several users, myself included, had some issues with Anki (recently migrated to uv). 
https://forums.ankiweb.net/t/bug-anki-25-07-fails-to-launch-on-linux/63475

zanieb came in and gave us pointers, including looking at our uv logs. 
https://github.com/ankitects/anki/pull/4074#issuecomment-3046992777
log: https://github.com/Grinkers/uv/pull/1#issuecomment-3047538135

The actual issue was that I had a system config in /etc/uv/uv.toml but uv wasn't giving useful feedback for its combining/unification.

A higher level issue is that there's nice logs, however logging is initialized after! We want to log files read, but need to read the files to know what log level to use.
https://github.com/astral-sh/uv/blob/7e48292fac968b015c4521e193b09e27af1d5c7b/crates/uv-settings/src/lib.rs#L68
https://github.com/astral-sh/uv/blob/7e48292fac968b015c4521e193b09e27af1d5c7b/crates/uv/src/lib.rs#L354

zanieb mentioned there's https://github.com/astral-sh/uv/issues/13123, so consider this a +1 to that.

## Result
The end of the output will be:
```
DEBUG Downloads disabled. Skipping...
DEBUG Released lock at `/tmp/uv-823c7b0e73da3e08.lock`
error: No interpreter found for Python 3.13.5 in managed installations
```

Sorry for the minuscule sized PR. Feel free to close if there's a bigger logging pass.

---

_Assigned to @zanieb by @zanieb on 2025-07-08 18:57_

---

_@zanieb approved on 2025-07-09 14:58_

---

_Label `error messages` added by @zanieb on 2025-07-09 14:59_

---

_Label `tracing` added by @zanieb on 2025-07-09 14:59_

---

_Renamed from "Debug message when skipping Python downloads." to "Add debug message when skipping Python downloads" by @zanieb on 2025-07-09 14:59_

---

_Merged by @zanieb on 2025-07-09 15:56_

---

_Closed by @zanieb on 2025-07-09 15:56_

---

_Branch deleted on 2025-07-09 16:04_

---
