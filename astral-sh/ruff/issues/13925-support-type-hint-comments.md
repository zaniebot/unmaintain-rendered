```yaml
number: 13925
title: Support type-hint comments
type: issue
state: closed
author: FFY00
labels: []
assignees: []
created_at: 2024-10-25T11:57:31Z
updated_at: 2024-10-25T14:23:32Z
url: https://github.com/astral-sh/ruff/issues/13925
synced_at: 2026-01-12T15:54:53Z
```

# Support type-hint comments

---

_@FFY00_

It would be nice if Ruff supported type-hint comments. These were in use before the type hint syntax was added to Python, which happened in 3.6.

While most projects have moved to syntax type-hints, there are projects with these still around. There may be a couple of reasons, like the project still haven't migrated to the new format, or the project needing to target older Python versions that do not support the new syntax.
For example, I am currently working on a project that introspects Python installations, and as such, I am maintaining a script that needs to run on older versions.

Example:
```python
if False:  # TYPE_CHECKING
    from typing import Any


def generate_data(schema_version: str):  # type: (str) -> dict[str, Any]
    ...
```

At the moment, Ruff issues F401 ("imported but unused") for the import in the example, which is incorrect, as `Any` is used in the type-hint comment.

That said, I do realize this doesn't have that many users, so it'd be totally understandable if you opt to not support it.
If it helps, I can look into implementing this myself, as I'd like to get more familiarized with the Ruff codebase.

---

_Comment by @AlexWaygood on 2024-10-25 14:21_

Thanks for the issue. Going to close as a duplicate of #1619 and #10501, however :-)

---

_Closed by @AlexWaygood on 2024-10-25 14:21_

---

_Comment by @FFY00 on 2024-10-25 14:23_

Ah, sorry! I did search for this on the issue tracker, but didn't notice those issues.

---
