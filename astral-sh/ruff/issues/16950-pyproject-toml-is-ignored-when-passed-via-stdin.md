```yaml
number: 16950
title: "`pyproject.toml` is ignored when passed via stdin"
type: issue
state: closed
author: InSyncWithFoo
labels:
  - bug
assignees: []
created_at: 2025-03-24T14:41:30Z
updated_at: 2025-03-27T16:01:46Z
url: https://github.com/astral-sh/ruff/issues/16950
synced_at: 2026-01-12T15:54:55Z
```

# `pyproject.toml` is ignored when passed via stdin

---

_@InSyncWithFoo_

This works:

```shell
$ cat pyproject.toml
[project]
name = 1

$ ruff check --isolated --select RUF200 pyproject.toml
pyproject.toml:2:8: RUF200 Failed to parse pyproject.toml: invalid type: integer `1`, expected a string
  |
1 | [project]
2 | name = 1
  |        ^ RUF200
  |

Found 1 error.
```

This doesn't:

```shell
$ ruff check --isolated --select RUF200 --stdin-filename pyproject.toml -
[project]
name = 1
All checks passed!
```

These two commands should work similarly.


---

_Comment by @InSyncWithFoo on 2025-03-24 14:50_

I think the server also doesn't report `RUF200` when a `pyproject.toml` is opened either.

---

_Comment by @MichaReiser on 2025-03-24 15:23_

> I think the server also doesn't report `RUF200` when a `pyproject.toml` is opened either.

Yes, the server doesn't run on `toml` files

---

_Label `bug` added by @MichaReiser on 2025-03-24 15:24_

---

_Closed by @MichaReiser on 2025-03-27 16:01_

---
