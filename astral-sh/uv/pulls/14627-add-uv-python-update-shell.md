```yaml
number: 14627
title: "Add `uv python update-shell`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/python-update-shell
created_at: 2025-07-15T14:47:33Z
updated_at: 2025-07-15T18:47:04Z
url: https://github.com/astral-sh/uv/pull/14627
synced_at: 2026-01-12T16:11:19Z
```

# Add `uv python update-shell`

---

_@zanieb_

Part of #14296 

This is the same as `uv tool update-shell` but handles the case where the Python bin directory is configured to a different path.

```
❯ UV_PYTHON_BIN_DIR=/tmp/foo cargo run -q -- python install --preview 3.13.3
Installed Python 3.13.3 in 1.75s
 + cpython-3.13.3-macos-aarch64-none
warning: `/tmp/foo` is not on your PATH. To use installed Python executables, run `export PATH="/tmp/foo:$PATH"` or `uv python update-shell`.
❯ UV_PYTHON_BIN_DIR=/tmp/foo cargo run -q -- python update-shell
Created configuration file: /Users/zb/.zshenv
Restart your shell to apply changes
❯ cat /Users/zb/.zshenv
# uv
export PATH="/tmp/foo:$PATH"
❯ UV_TOOL_BIN_DIR=/tmp/bar cargo run -q -- tool update-shell
Updated configuration file: /Users/zb/.zshenv
Restart your shell to apply changes
❯ cat /Users/zb/.zshenv
# uv
export PATH="/tmp/foo:$PATH"

# uv
export PATH="/tmp/bar:$PATH"
```

---

_Marked ready for review by @zanieb on 2025-07-15 18:46_

---

_Merged by @zanieb on 2025-07-15 18:47_

---

_Closed by @zanieb on 2025-07-15 18:47_

---

_Branch deleted on 2025-07-15 18:47_

---
