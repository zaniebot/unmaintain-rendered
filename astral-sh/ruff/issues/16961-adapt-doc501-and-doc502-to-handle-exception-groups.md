```yaml
number: 16961
title: "Adapt `DOC501` and `DOC502` to handle exception groups"
type: issue
state: open
author: gtkacz
labels:
  - rule
  - docstring
assignees: []
created_at: 2025-03-25T00:54:08Z
updated_at: 2025-03-25T23:23:11Z
url: https://github.com/astral-sh/ruff/issues/16961
synced_at: 2026-01-12T15:54:55Z
```

# Adapt `DOC501` and `DOC502` to handle exception groups

---

_@gtkacz_

### Summary

[`DOC501`](https://docs.astral.sh/ruff/rules/docstring-missing-exception/#docstring-missing-exception-doc501) and [`DOC502`](https://docs.astral.sh/ruff/rules/docstring-extraneous-exception/#docstring-extraneous-exception-doc502) currently don't handle [exception groups](https://peps.python.org/pep-0654/) properly: ruff expects a `Raises: ExceptionGroup` instead of a `Raises: ExceptionRaisedByExceptionGroup`. Given that a `ExceptionGroup` is not an exception per-se but rather "a combination of multiple unrelated exceptions", I propose that:

1. There be a way to disable `DOC501` if the underlying `ExceptionGroup` exceptions are documented.
2. `DOC502` recognize exceptions that are explicitly raised by an `ExceptionGroup` are to be documented in a `Raises:` section.

Example: https://play.ruff.rs/ff28a610-a275-4d88-a910-69e1b3ac86be

### Version

ruff 0.11.2 (4773878ee 2025-03-21)

---

_Label `rule` added by @dylwil3 on 2025-03-25 12:02_

---

_Label `docstring` added by @AlexWaygood on 2025-03-25 14:38_

---

_Comment by @gtkacz on 2025-03-25 23:23_

FYI I raised an issue with Google to officially determine how exception groups should be documented, which should be useful here: google/styleguide#907

---
