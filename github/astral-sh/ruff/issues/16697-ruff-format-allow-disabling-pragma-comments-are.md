---
number: 16697
title: "ruff format: allow disabling \"Pragma comments are ignored when computing line width\""
type: issue
state: closed
author: gotmax23
labels:
  - formatter
assignees: []
created_at: 2025-03-13T05:04:37Z
updated_at: 2025-03-13T11:23:22Z
url: https://github.com/astral-sh/ruff/issues/16697
synced_at: 2026-01-07T13:12:16-06:00
---

# ruff format: allow disabling "Pragma comments are ignored when computing line width"

---

_Issue opened by @gotmax23 on 2025-03-13 05:04_

See https://docs.astral.sh/ruff/formatter/black/#pragma-comments-are-ignored-when-computing-line-width. This is a divergence from black and causes lines that were previously wrapped so that pragma comments are one a separated line to be put back to together into a line that exceeds my preferred line length. Would it be possible to make this configurable?

```diff
--- src/fedrq/backends/dnf/backend/__init__.py
+++ src/fedrq/backends/dnf/backend/__init__.py
@@ -88,9 +88,7 @@
     def load_filelists(self, enable: bool = True) -> None:
         # Old versions of dnf always load filelists
         try:
-            types: list[str] = (
-                self.conf.optional_metadata_types
-            )  # pyright: ignore[reportAssignmentType]
+            types: list[str] = self.conf.optional_metadata_types  # pyright: ignore[reportAssignmentType]
         except AttributeError:
             return
         if enable:

```

---

_Label `formatter` added by @MichaReiser on 2025-03-13 11:21_

---

_Comment by @MichaReiser on 2025-03-13 11:23_

I'll merge this with https://github.com/astral-sh/ruff/issues/11941, which asks for an option to customize what `ruff format` considers to be a pragma comment. In your case, you would remove all pragma comments

---

_Closed by @MichaReiser on 2025-03-13 11:23_

---

_Closed by @MichaReiser on 2025-03-13 11:23_

---
