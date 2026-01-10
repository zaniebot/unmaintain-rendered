```yaml
number: 1032
title: T100 should not hit on import builtins
type: issue
state: closed
author: LefterisJP
labels: []
assignees: []
created_at: 2022-12-04T12:42:09Z
updated_at: 2022-12-04T15:13:18Z
url: https://github.com/astral-sh/ruff/issues/1032
synced_at: 2026-01-10T12:09:58Z
```

# T100 should not hit on import builtins

---

_Issue opened by @LefterisJP on 2022-12-04 12:42_

Seen with `ruff==0.0.155`

Simply doing an import builtins seems to hit us with `file:1:1 - T100 Import for `builtins` found


This should not be happening as there is legitimate reasons to import that module.

Seems to be a bug in flake-debugger 3.1.1
![2022-12-04_13-40](https://user-images.githubusercontent.com/1658405/205491023-cc85bcbe-29b4-4df9-9ea7-2378e1de6caf.png)

A reason to import builtins can be seen here: https://github.com/python/mypy/issues/14245

Got to that mypy problem actually by switching to PEP585 syntax thanks to ruff autofix :)

---

_Label `enhancement` added by @charliermarsh on 2022-12-04 14:45_

---

_Comment by @charliermarsh on 2022-12-04 14:45_

Yeah flagging `import builtins` is bad and far too strict. Will fix.

---

_Closed by @charliermarsh on 2022-12-04 15:13_

---
