```yaml
number: 13424
title: "Direct dependency warning mentions `--resolution lowest` when using `lowest-direct`"
type: issue
state: closed
author: nathanjmcdougall
labels:
  - bug
assignees: []
created_at: 2025-05-13T05:23:07Z
updated_at: 2025-05-13T07:25:06Z
url: https://github.com/astral-sh/uv/issues/13424
synced_at: 2026-01-10T03:41:47Z
```

# Direct dependency warning mentions `--resolution lowest` when using `lowest-direct`

---

_Issue opened by @nathanjmcdougall on 2025-05-13 05:23_

### Summary

This warning appears when failing to set lower bounds on dependencies:
```
warning: The direct dependency `click` is unpinned. Consider setting a lower bound when using `--resolution lowest` to avoid using outdated versions.
```

This occurs when using `--resolution lowest-direct`, which could be misleading since the message implies `--resolution lowest` is being used.

**Simple reproducer**
1. `uv init --lib`
2. Add an unpinned dep; e.g. `dependencies = ["click"]`
3. Run `uv export --resolution lowest-direct > requirements.txt`




### Platform

Windows 10 x86_64

### Version

uv 0.7.3 (3c413f74b 2025-05-07)

### Python version

Python 3.13.2

---

_Label `bug` added by @nathanjmcdougall on 2025-05-13 05:23_

---

_Renamed from "`direct dependency` warning mentions `--resolution lowest` when using `lowest-direct`" to "Direct dependency warning mentions `--resolution lowest` when using `lowest-direct`" by @nathanjmcdougall on 2025-05-13 05:23_

---

_Assigned to @konstin by @konstin on 2025-05-13 07:16_

---

_Closed by @konstin on 2025-05-13 07:25_

---

_Closed by @konstin on 2025-05-13 07:25_

---
