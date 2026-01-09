---
number: 8859
title: "docstring code formatter: add support for reStructuredText Python code snippets"
type: issue
state: closed
author: BurntSushi
labels:
  - docstring
  - formatter
assignees: []
created_at: 2023-11-27T17:04:09Z
updated_at: 2023-12-05T19:14:46Z
url: https://github.com/astral-sh/ruff/issues/8859
synced_at: 2026-01-07T13:12:15-06:00
---

# docstring code formatter: add support for reStructuredText Python code snippets

---

_Issue opened by @BurntSushi on 2023-11-27 17:04_

The initial implementation of the docstring code snippet formatter in #8811 only reformats doctest code snippets. This issue tracks the work for making it also reformat reStructuredText Python code snippets. Work should probably proceed by adding a new `CodeExampleKind`:

https://github.com/astral-sh/ruff/blob/d9845a2628d25cba3fa21bb53ad78ad377128aa2/crates/ruff_python_formatter/src/expression/string.rs#L1373-L1385

---

_Label `formatter` added by @BurntSushi on 2023-11-27 17:04_

---

_Renamed from "docstring code formatter: add support for reStructuredText code snippets" to "docstring code formatter: add support for reStructuredText Python code snippets" by @BurntSushi on 2023-11-27 17:04_

---

_Referenced in [astral-sh/ruff#8811](../../astral-sh/ruff/pulls/8811.md) on 2023-11-27 17:05_

---

_Referenced in [astral-sh/ruff#7146](../../astral-sh/ruff/issues/7146.md) on 2023-11-27 17:05_

---

_Label `docstring` added by @MichaReiser on 2023-11-27 23:29_

---

_Added to milestone `Formatter: Stable` by @BurntSushi on 2023-11-29 13:21_

---

_Referenced in [astral-sh/ruff#8950](../../astral-sh/ruff/pulls/8950.md) on 2023-12-01 16:39_

---

_Referenced in [astral-sh/ruff#9003](../../astral-sh/ruff/pulls/9003.md) on 2023-12-05 00:11_

---

_Closed by @BurntSushi on 2023-12-05 19:14_

---
