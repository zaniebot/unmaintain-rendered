```yaml
number: 9911
title: "`pyproject.toml` TOML parse error/warning shown twice"
type: issue
state: open
author: edmorley
labels:
  - error messages
assignees: []
created_at: 2024-12-15T13:36:33Z
updated_at: 2024-12-15T13:52:38Z
url: https://github.com/astral-sh/uv/issues/9911
synced_at: 2026-01-12T16:00:02Z
```

# `pyproject.toml` TOML parse error/warning shown twice

---

_@edmorley_

If `pyproject.toml` contains invalid TOML, eg:

```
[project]
name "testcase"
version = "0.0.0"
requires-python = ">=3.13"
dependencies = []
```

(Which is missing the `=` for the `name` field)

Then the resultant error message is duplicated - first as a warning then an error - rather than only being shown once:

```
$ uv sync
warning: Failed to parse `pyproject.toml` during settings discovery:
  TOML parse error at line 2, column 6
    |
  2 | name "testcase"
    |      ^
  expected `.`, `=`

error: Failed to parse: `pyproject.toml`
  Caused by: TOML parse error at line 2, column 6
  |
2 | name "testcase"
  |      ^
expected `.`, `=`
```

This is not high priority (and I imagine probably one of those awkward things to fix without refactoring a bunch), so feel free to wontfix if preferred.

```
$ uv --version
uv 0.5.9 (Homebrew 2024-12-13)
```

---

_Label `error messages` added by @charliermarsh on 2024-12-15 13:44_

---

_Comment by @charliermarsh on 2024-12-15 13:45_

Yeah I hate this... It's hard to avoid since these errors happen "far away" from one another but I hate it.

---

_Comment by @edmorley on 2024-12-15 13:52_

I suppose one option would be to make `pyproject.toml` TOML parse errors fatal during settings discovery instead? (Rather than trying to defer the warning until later to avoid a warning+error)

---
