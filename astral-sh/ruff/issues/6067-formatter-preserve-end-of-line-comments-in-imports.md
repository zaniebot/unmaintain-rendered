```yaml
number: 6067
title: "Formatter: Preserve end-of-line comments in imports"
type: issue
state: closed
author: konstin
labels:
  - formatter
assignees: []
created_at: 2023-07-25T11:11:01Z
updated_at: 2023-08-16T07:08:37Z
url: https://github.com/astral-sh/ruff/issues/6067
synced_at: 2026-01-12T15:54:45Z
```

# Formatter: Preserve end-of-line comments in imports

---

_@konstin_

We turn dangling end-of-line comments in the parentheses of import-from into own line comments instead of preserving them

black:
```python
from .fields import (  # NOQA
    GeometryField,
)
```

ours:
```python
from .fields import (
    # NOQA
    GeometryField,
)
```

---

_Label `formatter` added by @konstin on 2023-07-25 11:11_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-31 20:38_

---

_Comment by @charliermarsh on 2023-07-31 21:07_

Relatedly, this:

```python
from foo import bar, bop, bar, bop, bar, bop, bar, bop, bar, bop, bar, bop, bar, bop, bar, bop # baz
```

Gets wrapped as:

```python
from foo import (
    bar,
    bop,
    bar,
    bop,
    bar,
    bop,
    bar,
    bop,
    bar,
    bop,
    bar,
    bop,
    bar,
    bop,
    bar,
    bop,
)  # baz
```

---

_Comment by @charliermarsh on 2023-07-31 21:08_

We already solved these problems for the `isort` module, but it's probably hard to reuse what we built there 🤔 

---

_Comment by @konstin on 2023-08-01 09:21_

I've been thinking about reusing comment placement logic in the linter, it would allow e.g. better comment handling in edits

---

_Closed by @charliermarsh on 2023-08-01 18:58_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-16 07:08_

---
