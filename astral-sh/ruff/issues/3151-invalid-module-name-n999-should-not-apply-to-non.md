```yaml
number: 3151
title: "invalid-module-name (N999) should not apply to non-`*.py` files"
type: issue
state: closed
author: andersk
labels:
  - bug
assignees: []
created_at: 2023-02-23T00:16:13Z
updated_at: 2023-02-23T00:38:48Z
url: https://github.com/astral-sh/ruff/issues/3151
synced_at: 2026-01-12T15:54:43Z
```

# invalid-module-name (N999) should not apply to non-`*.py` files

---

_@andersk_

A script that isn’t named `*.py` ([example](https://github.com/zulip/zulip/blob/6.1/scripts/restart-server)) must be intended to be invoked from the command line, not imported as a module. So it doesn’t matter if its name is an invalid module name, and we should not emit

```
scripts/restart-server:1:1: N999 Invalid module name: 'restart-server'
```

Cc @sbrugman (#2855)

By the way, although we document this rule as being from pep8-naming, it isn’t; it’s from [flake8-module-name](https://pypi.org/project/flake8-module-name/).

---

_Comment by @charliermarsh on 2023-02-23 00:18_

Thanks. I can fix this real quick.

---

_Label `bug` added by @charliermarsh on 2023-02-23 00:32_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-23 00:34_

---

_Closed by @charliermarsh on 2023-02-23 00:38_

---
