```yaml
number: 19196
title: EXE003 Configuration of allowable interpreters
type: issue
state: open
author: jakeanq
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2025-07-08T08:09:37Z
updated_at: 2025-07-08T13:13:23Z
url: https://github.com/astral-sh/ruff/issues/19196
synced_at: 2026-01-12T15:54:56Z
```

# EXE003 Configuration of allowable interpreters

---

_@jakeanq_

### Summary

It would be useful to be able to configure the list of interpreters that EXE003 will accept.

My current use-case is supporting `#!/usr/bin/env hython` to use Houdini's Python environment (https://www.sidefx.com/docs/houdini/hom/commandline.html#hython).

Some other advantages of making it configurable could include:
 * Removing one of the defaults from the list if it's not valid (eg to disallow `pytest` in the shebang).
 * Allowing other interpreters such as `pypy`.
 * If a regex is used as the config option, it could be used to enforce specific formats of shebang, eg to only allow `^#!/usr/bin/env python3?$`.

---

_Label `configuration` added by @ntBre on 2025-07-08 13:13_

---

_Label `needs-decision` added by @ntBre on 2025-07-08 13:13_

---
