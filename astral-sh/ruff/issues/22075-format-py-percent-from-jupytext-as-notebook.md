```yaml
number: 22075
title: "format `py:percent` from `jupytext` as notebook"
type: issue
state: closed
author: ogauthe
labels:
  - question
assignees: []
created_at: 2025-12-19T10:28:38Z
updated_at: 2025-12-19T15:02:35Z
url: https://github.com/astral-sh/ruff/issues/22075
synced_at: 2026-01-12T15:54:58Z
```

# format `py:percent` from `jupytext` as notebook

---

_@ogauthe_

### Question

I am using notebooks with `jupytext` in the `py:percent` format. This allows to save notebooks as regular `.py` files and makes version control, linting and testing much more convenient.

Ideally, I would like to format these files with ruff, but preserve a few notebooks features. As mentioned in #7300 #8590 or #19980, notebooks need different rules, especially considering semicolons. Unfortunately I have note been able to enforce these properties on my `.py` notebooks.

Would it be possible to configure `ruff format` to preserve semicolons / use notebook conventions for specified `.py` files?

### Version

ruff 0.9.3

---

_Label `question` added by @ogauthe on 2025-12-19 10:28_

---

_Comment by @ntBre on 2025-12-19 14:50_

I think this is also related to #8800, which tracks general jupytext support.

---

_Closed by @MichaReiser on 2025-12-19 15:02_

---
