```yaml
number: 15919
title: "Allow `--isolated` with script execution"
type: issue
state: open
author: zanieb
labels:
  - enhancement
  - good first issue
assignees: []
created_at: 2025-09-17T19:48:23Z
updated_at: 2025-09-18T04:46:10Z
url: https://github.com/astral-sh/uv/issues/15919
synced_at: 2026-01-12T16:02:19Z
```

# Allow `--isolated` with script execution

---

_@zanieb_

Right now, we ignore the flag and display a warning

```
❯ uv run --isolated --script example.py
warning: `--isolated` is a no-op for Python scripts with inline metadata, which always run in isolation
```

but now that we changed scripts to use a stable path, we should allow `--isolated` to opt-out of that, e.g., for people in situations like https://github.com/astral-sh/uv/issues/15918

---

_Label `enhancement` added by @zanieb on 2025-09-17 19:48_

---

_Label `good first issue` added by @zanieb on 2025-09-17 20:21_

---

_Comment by @zanieb on 2025-09-17 20:21_

This isn't trivial, but I think it shouldn't be too hard.

---

_Comment by @mohammedgqudah on 2025-09-18 04:46_

created https://github.com/astral-sh/uv/pull/15924

---
