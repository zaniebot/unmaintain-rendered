```yaml
number: 20280
title: Relax DOC rules for private/inner methods
type: issue
state: open
author: cbornet
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2025-09-06T09:58:37Z
updated_at: 2025-09-08T19:54:59Z
url: https://github.com/astral-sh/ruff/issues/20280
synced_at: 2026-01-12T15:54:57Z
```

# Relax DOC rules for private/inner methods

---

_@cbornet_

### Summary

Currently the following code triggers DOC201 (missing return)

```python
def _my_helper_function() -> str
   """This is some helper function."""
    return "foo"
```

Whereas this doesn't trigger:

```python
def _my_helper_function() -> str
    return "foo"
```

It would be nice if we could put just a short description to the method without a full-blown docstring to private and inner functions/methods (ie not trigger on the first code snippet).

---

_Comment by @cbornet on 2025-09-06 10:19_

It is like automatically setting `ignore-one-line-docstrings = true` for non-visible methods.

---

_Comment by @ntBre on 2025-09-08 19:54_

This makes some sense to me, maybe as a separate configuration option. The `pydocstyle` (`D`) rules differentiate between private and public functions, but the `pydoclint` (`DOC`) rules don't.

---

_Label `configuration` added by @ntBre on 2025-09-08 19:54_

---

_Label `needs-decision` added by @ntBre on 2025-09-08 19:54_

---
