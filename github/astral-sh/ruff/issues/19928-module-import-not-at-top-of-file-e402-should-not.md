---
number: 19928
title: module-import-not-at-top-of-file (E402) should not warn about logging configuration
type: issue
state: open
author: JimDabell
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-08-15T14:09:32Z
updated_at: 2025-08-15T15:48:42Z
url: https://github.com/astral-sh/ruff/issues/19928
synced_at: 2026-01-07T13:12:16-06:00
---

# module-import-not-at-top-of-file (E402) should not warn about logging configuration

---

_Issue opened by @JimDabell on 2025-08-15 14:09_

### Summary

It’s frequently necessary to configure logging before importing a package that logs. For instance, if a package `foo`  logs `DEBUG` level events, you might want to write:

```python
import logging


logging.basicConfig(level=logging.DEBUG)


import foo
```

[The documentation notes:](https://docs.astral.sh/ruff/rules/module-import-not-at-top-of-file/)

> This rule makes an exception for both `sys.path` modifications (allowing for `sys.path.insert`, `sys.path.append`, etc.) and `os.environ` modifications between imports.

I believe this exception should also extend to logging configuration.

---

_Comment by @ntBre on 2025-08-15 15:48_

It's not quite a duplicate, but we're having a similar discussion on E402 in #19904. If we take a more general approach there, we could fold this issue in as well. `logging` probably fits better into the existing special cases since it's also from the stdlib, though.

---

_Label `rule` added by @ntBre on 2025-08-15 15:48_

---

_Label `needs-decision` added by @ntBre on 2025-08-15 15:48_

---

_Referenced in [astral-sh/ruff#19904](../../astral-sh/ruff/issues/19904.md) on 2025-08-15 15:51_

---

_Referenced in [astral-sh/ruff#20288](../../astral-sh/ruff/issues/20288.md) on 2025-09-08 19:45_

---

_Referenced in [astral-sh/ruff#21423](../../astral-sh/ruff/issues/21423.md) on 2025-11-13 15:29_

---
