```yaml
number: 8627
title: "Update CLI documentation for `--cache-dir`"
type: pull_request
state: merged
author: simonw
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-10-28T01:26:20Z
updated_at: 2024-10-28T02:01:21Z
url: https://github.com/astral-sh/uv/pull/8627
synced_at: 2026-01-12T16:08:24Z
```

# Update CLI documentation for `--cache-dir`

---

_@simonw_

Refs:
- #8626

## Summary

Current documentation incorrectly suggests that the macOS cache directory location is `$HOME/Library/Caches/uv`, but that changed in:

- #5806

Updates docs to say this instead:

> <p>Defaults to <code>$HOME/.cache/uv</code> on macOS, <code>$XDG_CACHE_HOME/uv</code> or <code>$HOME/.cache/uv</code> on Linux, and <code>%LOCALAPPDATA%\uv\cache</code> on Windows. The <code>uv cache dir</code> command will show the location of the cache directory.</p>

---

_Comment by @charliermarsh on 2024-10-28 01:40_

Thank you! These docs are auto-generated so I'll apply these changes to the source and re-generate.

---

_@charliermarsh approved on 2024-10-28 01:45_

---

_Comment by @charliermarsh on 2024-10-28 01:45_

(Tweaked lightly to match some copy we have on `uv tool dir`.)

---

_Label `documentation` added by @charliermarsh on 2024-10-28 01:45_

---

_Renamed from "Update docs for --cache-dir cache-dir" to "Update CLI documentation for `--cache-dir`" by @charliermarsh on 2024-10-28 01:45_

---

_Merged by @charliermarsh on 2024-10-28 02:01_

---

_Closed by @charliermarsh on 2024-10-28 02:01_

---
