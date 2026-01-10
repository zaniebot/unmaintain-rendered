```yaml
number: 12832
title: Support PEP 420 Namespace Packages in the uv build backend
type: issue
state: closed
author: konstin
labels:
  - build-backend
assignees: []
created_at: 2025-04-11T09:54:33Z
updated_at: 2025-06-13T15:03:53Z
url: https://github.com/astral-sh/uv/issues/12832
synced_at: 2026-01-10T03:32:45Z
```

# Support PEP 420 Namespace Packages in the uv build backend

---

_Issue opened by @konstin on 2025-04-11 09:54_

PEP 420 allow multiple packages to use a share top-level namespace. Roughly speaking, there could be a package `foo.core` (normalized to `foo-core`) that contains `foo/core/__init__.py`, while package `foo.bar-plugin` (normalized to `foo-bar-plugin`) that contains `foo/bar_plugin/__init__.py`. Neither contains a `foo/__init__.py`, and installing them makes them share the `import foo.` prefix (and the `foo` site packages directory).

---

_Label `build-backend` added by @konstin on 2025-04-11 09:54_

---

_Comment by @winstxnhdw on 2025-05-06 09:04_

Is there a workaround for this atm?

---

_Comment by @astrojuanlu on 2025-06-13 14:57_

https://github.com/astral-sh/uv/issues/3957#issuecomment-2970247729 can be closed?

---

_Comment by @konstin on 2025-06-13 15:03_

Yes!

Fixed by #13833, docs are on https://docs.astral.sh/uv/concepts/build-backend/.

---

_Closed by @konstin on 2025-06-13 15:03_

---
