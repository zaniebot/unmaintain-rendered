```yaml
number: 15283
title: "Remove `uv pip install` hint for build dep module not found errors"
type: issue
state: open
author: blueraft
labels: []
assignees: []
created_at: 2025-08-14T14:57:53Z
updated_at: 2025-08-14T14:59:56Z
url: https://github.com/astral-sh/uv/issues/15283
synced_at: 2026-01-10T03:32:46Z
```

# Remove `uv pip install` hint for build dep module not found errors

---

_Issue opened by @blueraft on 2025-08-14 14:57_

Ref: https://github.com/astral-sh/uv/pull/15252#discussion_r2276581336

This hint might be insufficient since the user will need to install all of the build dependencies for this to work correctly.

---

_Comment by @blueraft on 2025-08-14 14:59_

Although if a user is just using the pip interface without a toml file, it might be easier than the `extra-build-dependencies` option.

---
