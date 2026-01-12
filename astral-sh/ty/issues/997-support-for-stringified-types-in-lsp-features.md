```yaml
number: 997
title: Support for stringified types in LSP features
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-08-15T06:24:25Z
updated_at: 2025-08-15T15:46:32Z
url: https://github.com/astral-sh/ty/issues/997
synced_at: 2026-01-12T15:54:24Z
```

# Support for stringified types in LSP features

---

_@MichaReiser_

The LSP currently doesn't support any operations on stringified types. This includes:

* Go to definition/declaration/...
* rename: If a stringified type annotation contains a reference to a symbol that's renamed
* Hover
* semantic syntax highlighting `a | b` is highlighted as a string instead of an identifier, operator, identifier
* ...

What makes supporting stringified types tricky is that Ruff's parser parses them as string literal expressions and `ty_python_semantic` doesn't store the inferred types for it (except for the outermost expression). 

See https://github.com/astral-sh/ruff/pull/19905

---

_Label `server` added by @MichaReiser on 2025-08-15 06:24_

---

_Added to milestone `GA` by @MichaReiser on 2025-08-15 12:31_

---

_Closed by @MichaReiser on 2025-08-15 15:46_

---
