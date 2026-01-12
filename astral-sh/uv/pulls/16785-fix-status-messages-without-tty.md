```yaml
number: 16785
title: Fix status messages without TTY
type: pull_request
state: merged
author: rzblue
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-status-output
created_at: 2025-11-20T09:54:10Z
updated_at: 2025-11-20T14:33:13Z
url: https://github.com/astral-sh/uv/pull/16785
synced_at: 2026-01-12T16:12:26Z
```

# Fix status messages without TTY

---

_@rzblue_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

#12175 changed the behavior of `on_request_complete` when stderr is not a tty to output `Downloading`/`Uploading` (via `Direction::as_str`). This fixes it to output `Downloaded`/`Uploaded` again.

## Test Plan

Tested locally to verify new output.

Old:
```
$ uv sync --no-cache 2>&1 | tee /dev/null
Using CPython 3.14.0
Creating virtual environment at: .venv
Resolved 12 packages in 19ms
Downloading numpy (15.8MiB)
Downloading matplotlib (9.4MiB)
Downloading fonttools (4.6MiB)
Downloading pillow (6.7MiB)
Downloading kiwisolver (1.4MiB)
 Downloading kiwisolver
 Downloading fonttools
 Downloading pillow
 Downloading matplotlib
 Downloading numpy
```
New:
```
$ uv sync --no-cache 2>&1 | tee /dev/null
Using CPython 3.14.0
Creating virtual environment at: .venv
Resolved 12 packages in 3ms
Downloading numpy (15.8MiB)
Downloading fonttools (4.6MiB)
Downloading matplotlib (9.4MiB)
Downloading kiwisolver (1.4MiB)
Downloading pillow (6.7MiB)
 Downloaded kiwisolver
 Downloaded pillow
 Downloaded fonttools
 Downloaded matplotlib
 Downloaded numpy
```

---

_Label `bug` added by @konstin on 2025-11-20 14:13_

---

_Renamed from "Fix incorrect reporter completion output" to "Fix status messages without TTY" by @konstin on 2025-11-20 14:13_

---

_@konstin approved on 2025-11-20 14:13_

Thanks!

---

_Merged by @konstin on 2025-11-20 14:14_

---

_Closed by @konstin on 2025-11-20 14:14_

---

_Branch deleted on 2025-11-20 14:33_

---
