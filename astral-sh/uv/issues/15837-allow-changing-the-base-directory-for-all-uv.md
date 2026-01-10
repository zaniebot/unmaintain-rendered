---
number: 15837
title: "Allow changing the base directory for all uv storage, e.g., with `UV_ROOT_DIR`"
type: issue
state: open
author: zanieb
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2025-09-14T14:07:16Z
updated_at: 2025-12-18T17:37:01Z
url: https://github.com/astral-sh/uv/issues/15837
synced_at: 2026-01-10T01:26:00Z
---

# Allow changing the base directory for all uv storage, e.g., with `UV_ROOT_DIR`

---

_Issue opened by @zanieb on 2025-09-14 14:07_

Originally mentioned at https://github.com/astral-sh/uv/issues/11360#issuecomment-2943941428

This would simplify configuration for people who want to store all of uv's directories in a single location rather than the platform-specific defaults.

The alternative to this is setting several `XDG_*` values, which would affect software other than uv.

---

_Label `configuration` added by @zanieb on 2025-09-14 14:07_

---

_Label `needs-decision` added by @zanieb on 2025-09-14 14:07_

---

_Comment by @mekskendo-lhind on 2025-10-21 09:05_

Please add this soon, would save a lot of pain when managing UV installation on multiple instances.

As a work around for now I grabbed every variable that ends in `_DIR` from https://docs.astral.sh/uv/reference/environment/ and put it in a profile.d file:

```bash
sudo tee /etc/profile.d/uv.sh > /dev/null << 'EOF'
#!/bin/bash

# UV Base Directories
export UV_CACHE_DIR=/usr/local/bin/.uv/cache
export UV_CREDENTIALS_DIR=/usr/local/bin/.uv/credentials

# UV Binary Installation
export UV_INSTALL_DIR=/usr/local/bin/.uv/bin

# Python Management
export UV_PYTHON_INSTALL_DIR=/usr/local/bin/.uv/python/install
export UV_PYTHON_BIN_DIR=/usr/local/bin/.uv/python/bin
export UV_PYTHON_CACHE_DIR=/usr/local/bin/.uv/python/cache

# Tool Management
export UV_TOOL_DIR=/usr/local/bin/.uv/tools
export UV_TOOL_BIN_DIR=/usr/local/bin/.uv/tools/bin
EOF
```

```
sudo chmod 644 /etc/profile.d/uv.sh
```

```
source /etc/profile.d/uv.sh`
```

But this would be really annoying to maintain on multiple instances long term if `_DIR`s keep adding/removing/changing.


---

_Comment by @laundmo on 2025-12-18 17:37_

To anyone else reading this:
Note that the script shared above is likely wrong. You probably dont want `UV_INSTALL_DIR`, `UV_PYTHON_BIN_DIR` and `UV_TOOL_BIN_DIR` inside a .uv folder, and instead pointing at a folder on the `PATH`. Similarly, a global cache dir needs quite a bit of extra consideration, see 
- #5611 

---
