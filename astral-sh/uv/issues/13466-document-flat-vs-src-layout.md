---
number: 13466
title: Document flat vs src layout
type: issue
state: open
author: travis-leith
labels:
  - documentation
assignees: []
created_at: 2025-05-15T12:24:14Z
updated_at: 2025-08-15T15:10:10Z
url: https://github.com/astral-sh/uv/issues/13466
synced_at: 2026-01-10T01:25:34Z
---

# Document flat vs src layout

---

_Issue opened by @travis-leith on 2025-05-15 12:24_

### Question

Why not just libname/src?

If this has already been asked, then consider this a feature request to explain this in the documentation here: https://docs.astral.sh/uv/concepts/projects/workspaces/#workspace-layouts

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @travis-leith on 2025-05-15 12:24_

---

_Label `question` removed by @konstin on 2025-05-15 12:29_

---

_Label `documentation` added by @konstin on 2025-05-15 12:29_

---

_Comment by @zanieb on 2025-05-15 12:40_

In order for the library to be a proper Python module, it needs the directory to have the name. e.g., `<name>/__init__.py`. Moving that directory into `src/` stops you from accidentally importing it from the root directory of the project without installing it. See https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/ for more on that. `src/__init__.py` would drop the `<name>` module, breaking `import <name>`. There's a bit more context, and an experiment to make `src/__init__.py` "work" at https://github.com/astral-sh/uv/pull/11325.

---

_Renamed from "Why do library workspace elements have a structure of libname/src/libname?" to "Document flat vs src layout" by @konstin on 2025-05-15 19:38_

---

_Comment by @JimDabell on 2025-08-15 15:10_

See: #8178

---
