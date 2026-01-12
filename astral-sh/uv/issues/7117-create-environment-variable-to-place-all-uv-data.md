```yaml
number: 7117
title: Create environment variable to place all uv data under one directory
type: issue
state: open
author: bmarroquin
labels: []
assignees: []
created_at: 2024-09-06T05:55:46Z
updated_at: 2024-09-06T05:55:46Z
url: https://github.com/astral-sh/uv/issues/7117
synced_at: 2026-01-12T15:59:10Z
```

# Create environment variable to place all uv data under one directory

---

_@bmarroquin_

I want to move all UV related files into another drive on my system. Currently, i would need to set a few environment variables to get this to work. It would be nice to add an env var that can be used to move them all at once.

For example:
```
UV_ROOT
├── bin        # location of uv, uvx  if UV_ROOT exists during install
├── cache      # equivalent to UV_CACHE_DIR
├── python     # equivalent to UV_PYTHON_INSTALL_DIR
├── tools      # equivalent to UV_TOOL_DIR
├── tools_bin  # equivalent to UV_TOOL_DIR_BIN, could be merged with bin?
└── uv.toml    # equivalent to UV_CONFIG_FILE
```
If any of the more specific env vars exist, they should take precedence over the `UV_ROOT` sub directory.

---
