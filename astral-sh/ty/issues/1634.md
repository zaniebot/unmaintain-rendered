```yaml
number: 1634
title: Refactoring support
type: issue
state: open
author: pyscripter
labels:
  - server
assignees: []
created_at: 2025-11-25T21:07:20Z
updated_at: 2025-12-16T07:45:16Z
url: https://github.com/astral-sh/ty/issues/1634
synced_at: 2026-01-10T01:55:00Z
```

# Refactoring support

---

_Issue opened by @pyscripter on 2025-11-25 21:07_

This is a feature request.

Rename and auto-import functionality is already available, but it would be very useful to provide additional refactoring support.  At a minimum, support for the Jedi refactorings could be provided, so that users can switch to ty from jedi-based LSP without loss of functionality:

- extract variable
- extract function
- inline

This would help faster adoption.

Of course, ideally, more refactorings could be supported.  See for example the [relevant issue](https://github.com/facebook/pyrefly/issues/364) of [pyrefly](https://github.com/facebook/pyrefly)

---

_Label `server` added by @AlexWaygood on 2025-11-25 23:08_

---

_Comment by @asukaminato0721 on 2025-12-16 07:45_

for extract function, this can be ref https://github.com/facebook/pyrefly/commit/39ec3446b2f5eeebbef4c6d4d6a99377b6295aaf

---
