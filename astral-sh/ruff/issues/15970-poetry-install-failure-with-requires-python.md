```yaml
number: 15970
title: "Poetry install failure with `requires-python`"
type: issue
state: closed
author: justRishi
labels:
  - question
assignees: []
created_at: 2025-02-05T14:13:54Z
updated_at: 2025-02-07T10:11:26Z
url: https://github.com/astral-sh/ruff/issues/15970
synced_at: 2026-01-12T15:54:55Z
```

# Poetry install failure with `requires-python`

---

_@justRishi_

### Description

Refers to the configuration option documented here [ruff configuration](https://docs.astral.sh/ruff/tutorial/#configuration) requires-python.
ruff version used = 0.8.6 in combination with poetry version = 1.8.5 on ubuntu 24.04

in pyproject.toml the following results in a problem when the project is used local in another python project using `pip install -e ../<some-poetry- project>` or `poetry run pip install -e <some-poetry-managed-project>

```
[project]
requires-python = ">=3.11"
```

removing this(requires-python ) solves the problem, perhaps [project]  is invalid for pyproject.toml ? 


relevant parts of pyproject.py
```
[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"

[tool.poetry]
name = "xxx"

[project]
requires-python = ">=3.11"
```

error stack is  (project name is not missing by the way) : 

```
  × Preparing editable metadata (pyproject.toml) did not run successfully.
  │ exit code: 1
  ╰─> [17 lines of output]
      Traceback (most recent call last):
        File "..../dq-api/.venv/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 389, in <module>
          main()
        File "....../.venv/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 373, in main
          json_out["return_val"] = hook(**hook_input["kwargs"])
                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "..../.venv/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 209, in prepare_metadata_for_build_editable
          return hook(metadata_directory, config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/tmp/pip-build-env-b04i3amd/overlay/lib/python3.12/site-packages/poetry/core/masonry/api.py", line 42, in prepare_metadata_for_build_wheel
          poetry = Factory().create_poetry(Path().resolve(), with_groups=False)
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/tmp/pip-build-env-b04i3amd/overlay/lib/python3.12/site-packages/poetry/core/factory.py", line 59, in create_poetry
          raise RuntimeError("The Poetry configuration is invalid:\n" + message)
      RuntimeError: The Poetry configuration is invalid:
        - project must contain ['name'] properties
      
      [end of output]
```

---

_Comment by @MichaReiser on 2025-02-05 14:18_

The `project.name` property is required in a `pyproject.toml` if it has a `[project]` table and it does seem missing in the configuration you shared.

```toml
[project]
name = "project-name"
requires-python = ">=3.11"
```

You can otherwise use the [`target-version`](https://docs.astral.sh/ruff/settings/#unsafe-fixes) setting:

```toml
[tool.ruff]
target-version = "py311"
```

---

_Label `question` added by @MichaReiser on 2025-02-05 14:18_

---

_Renamed from "0.8.6" to "Poetry install failure with `requires-python`" by @MichaReiser on 2025-02-05 16:13_

---

_Closed by @MichaReiser on 2025-02-07 10:11_

---
