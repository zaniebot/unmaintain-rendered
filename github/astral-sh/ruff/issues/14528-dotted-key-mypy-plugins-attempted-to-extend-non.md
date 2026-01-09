---
number: 14528
title: "dotted key `mypy.plugins` attempted to extend non-table type (array)"
type: issue
state: closed
author: skhaz
labels:
  - question
assignees: []
created_at: 2024-11-22T13:00:34Z
updated_at: 2024-11-22T15:22:46Z
url: https://github.com/astral-sh/ruff/issues/14528
synced_at: 2026-01-07T13:12:16-06:00
---

# dotted key `mypy.plugins` attempted to extend non-table type (array)

---

_Issue opened by @skhaz on 2024-11-22 13:00_

I have this `pyproject.toml`

```toml
[tool.pyright]
venvPath = "."
venv = ".venv"

[tool.ruff]
line-length = 120
target-version = "py312"

[tool.ruff.lint]
select = ["F", "I"]

[tool.ruff.lint.isort]
force-single-line = true

[mypy]
plugins = ["mypy_django_plugin.main"]

[mypy.plugins.django-stubs]
django_settings_module = "gatekeeper.settings"
```

Which works fine.

When I run

```shell
ruff format . && ruff check --fix .
```

I get the following error

```
ruff failed
  Cause: Failed to parse /opt/workspace/company/project/pyproject.toml
  Cause: TOML parse error at line 18, column 1
   |
18 | [mypy.plugins.django-stubs]
   | ^
invalid table header
dotted key `mypy.plugins` attempted to extend non-table type (array)
```

---

_Comment by @MichaReiser on 2024-11-22 15:03_

Hi @skhaz 

I think you want to use `[tool.mypy]` according to [mypy's documentation](https://mypy.readthedocs.io/en/stable/config_file.html#using-a-pyproject-toml-file). 

Ideally, Ruff would accept invalid `pyproject.toml` files for keys unrelated to Ruff.

---

_Label `question` added by @MichaReiser on 2024-11-22 15:03_

---

_Comment by @skhaz on 2024-11-22 15:05_

Same issue. The original configuration comes from Django Stubs.

```
ruff format . && ruff check --fix .
ruff failed
  Cause: Failed to parse /opt/workspace/kashmir/gatekeeper/pyproject.toml
  Cause: TOML parse error at line 18, column 1
   |
18 | [tool.mypy.plugins.django-stubs]
   | ^
invalid table header
dotted key `tool.mypy.plugins` attempted to extend non-table type (array)
```

---

_Comment by @MichaReiser on 2024-11-22 15:20_

Oh, I see the issue. This is not valid toml. A value can't be a list (here `mypy.plugins`) and a table (`[mypy.plugins.django-stubs]`) at the same time. `mypy.plugins` becomes a table because of the `plugins` *dot* `django_stubs`. 

You can validate whether your toml is structurally valid using https://www.toml-lint.com/

---

_Comment by @MichaReiser on 2024-11-22 15:21_

This should probably something like 

```toml

[mypy.plugins.mypy_django_plugin]
main = true # not sure what the value should be here

[mypy.plugins.django-stubs]
django_settings_module = "gatekeeper.settings"

---

_Comment by @skhaz on 2024-11-22 15:22_

Thank you so much. I will let them know, as I got that configuration from their documentation and assumed it was correct, thinking the issue was with Ruff.

---

_Closed by @skhaz on 2024-11-22 15:22_

---
