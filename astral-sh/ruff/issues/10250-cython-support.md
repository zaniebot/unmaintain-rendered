```yaml
number: 10250
title: Cython support
type: issue
state: open
author: MichaReiser
labels:
  - core
assignees: []
created_at: 2024-03-06T16:22:12Z
updated_at: 2024-08-13T08:01:11Z
url: https://github.com/astral-sh/ruff/issues/10250
synced_at: 2026-01-10T11:09:52Z
```

# Cython support

---

_Issue opened by @MichaReiser on 2024-03-06 16:22_

Originally requested in https://github.com/astral-sh/ruff/issues/1079

Requires a custom parse mode and AST extensions to support Cython specific syntax. See https://cython.readthedocs.io/en/latest/src/userguide/language_basics.html

---

_Label `core` added by @MichaReiser on 2024-03-06 16:22_

---

_Comment by @MarcoGorelli on 2024-03-06 16:31_

They provide StringParseContext, which gives something like an AST but with lots of missing parts (I had to do quite some working around it in cython-lint...sharing in case it's useful as a starting point: https://github.com/MarcoGorelli/cython-lint/blob/9e1d65daed6a23ff32e1893b749607432fd6d8f1/cython_lint/cython_lint.py#L374-L689)

---

_Comment by @carmocca on 2024-06-01 10:12_

`isort` 5 introduced support for Cython: https://pycqa.github.io/isort/docs/major_releases/introducing_isort_5.html. This could be an easier first step towards full support.

---

_Comment by @KelSolaar on 2024-08-13 08:00_

I would also like to see the formatter working with Cython.

(Pretty Please)

---
