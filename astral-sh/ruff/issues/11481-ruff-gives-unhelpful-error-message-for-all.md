```yaml
number: 11481
title: Ruff gives unhelpful error message for all commands when pyproject.toml is invalid
type: issue
state: closed
author: mscheifer
labels:
  - question
assignees: []
created_at: 2024-05-21T02:57:38Z
updated_at: 2024-06-04T03:56:05Z
url: https://github.com/astral-sh/ruff/issues/11481
synced_at: 2026-01-12T15:54:51Z
```

# Ruff gives unhelpful error message for all commands when pyproject.toml is invalid

---

_@mscheifer_

When my `pyproject.toml` file is invalid, every `ruff` command gives the same somewhat confusing error message.
```shell
$ poetry run ruff check

Invalid statement (at line 9, column 1)
$ poetry run ruff --help

Invalid statement (at line 9, column 1)
$ poetry run ruff --version

Invalid statement (at line 9, column 1)
$ poetry run ruff server --preview

Invalid statement (at line 9, column 1)
```

My `pyproject.toml` (reduced down for simplicity) looks like:
```toml
[tool.poetry]
package-mode = false

[tool.poetry.dependencies]
python = "^3.10"

[tool.poetry.group.dev.dependencies]
ruff = "^0.4.4"
<<<<<<< HEAD
hypothesis = "^6.102.4"
=======
pytest = "^8.2.1"
>>>>>>> main

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

The error message doesn't make it clear that this is caused by the`pyproject.toml` file. I think the message should say
```
Failed to parse pyproject.toml. Aborting
Invalid statement (at line 9, column 1)
```
or something.

Less importantly, it would be nice if some commands like `--help` and `--version` still worked even when it can't parse `pyproject.toml`.

---

_Comment by @charliermarsh on 2024-05-21 19:12_

In my terminal I see this:

```
❯ ruff check
ruff failed
  Cause: Failed to parse /Users/crmarsh/workspace/ruff/pyproject.toml
  Cause: TOML parse error at line 123, column 1
    |
123 | <<<<<<< HEAD
    | ^
invalid key
```

Which seems pretty good...

I think this is a Poetry error? If I run `poetry run`:

```
❯ poetry run ruff

Invalid statement (at line 123, column 1)
```

---

_Label `question` added by @charliermarsh on 2024-05-21 19:12_

---

_Comment by @mscheifer on 2024-05-21 19:51_

Ahh you're right. I think this happens for any `poetry run` command.
```
$ poetry run echo foo

Invalid statement (at line 9, column 1)
```

---

_Closed by @mscheifer on 2024-05-21 19:51_

---

_Closed by @mscheifer on 2024-05-21 20:03_

---

_Comment by @mscheifer on 2024-06-04 03:43_

This error message should be improved in newer versions of poetry https://github.com/python-poetry/poetry-core/commit/1459c57c825cda5f9a286bffaf233b77583427b9

---

_Comment by @charliermarsh on 2024-06-04 03:56_

Thanks @mscheifer!

---
