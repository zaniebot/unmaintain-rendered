---
number: 17572
title: Cursor jumps to end of file when formatting files with large tuples
type: issue
state: open
author: rintrint
labels:
  - formatter
assignees: []
created_at: 2025-04-23T06:19:42Z
updated_at: 2025-04-23T06:46:19Z
url: https://github.com/astral-sh/ruff/issues/17572
synced_at: 2026-01-07T13:12:16-06:00
---

# Cursor jumps to end of file when formatting files with large tuples

---

_Issue opened by @rintrint on 2025-04-23 06:19_

### Summary

When formatting Python files with large tuples (>20K chars), the cursor jumps to the end of file. This only happens when:
1. The tuple is very large (>20K characters)
2. The formatter makes significant changes, causing a delay
3. The cursor is positioned **INSIDE** the large tuple when formatting is triggered

I've tested that:
- Reducing tuple size to <20K chars fixes the issue
- Files with 30K+ char tuples but minimal formatting changes don't have this issue

The problem seems related to both tuple size AND amount of reformatting needed.

Reproducible with this file:
https://github.com/MMD-Blender/blender_mmd_tools/blob/6ae13d6039763b6813832622c13da464983e93b3/mmd_tools/m17n.py

### Version

VS Code: 1.99.3
Ruff extension: 2025.22.0
OS: Windows_NT x64 10.0.19045

---

_Renamed from "Cursor jumps to end of file when formatting files with large tuple" to "Cursor jumps to end of file when formatting files with large tuples" by @rintrint on 2025-04-23 06:23_

---

_Label `formatter` added by @MichaReiser on 2025-04-23 06:24_

---

_Comment by @MichaReiser on 2025-04-23 06:25_

Thanks for the report. The cursor positioning is currently handled by VS code. We may need to implement https://github.com/astral-sh/ruff/issues/10179 to improve the positioning.

---

_Comment by @rintrint on 2025-04-23 06:38_

Thanks for the info about #10179.
I tested with Black Formatter for comparison and found that while it also moves the cursor, it only shifts it slightly rather than jumping to the end of file. This is much more usable behavior.
However, Black Formatter is significantly slower when formatting this file with a large tuple.

---

_Comment by @MichaReiser on 2025-04-23 06:46_

Interesting, nice find. Another possibility here is that using a different diff algorithm when computing the text edits between the formatted and unformatted string might produce better results.

---

_Referenced in [astral-sh/ty#71](../../astral-sh/ty/issues/71.md) on 2025-05-05 13:44_

---
