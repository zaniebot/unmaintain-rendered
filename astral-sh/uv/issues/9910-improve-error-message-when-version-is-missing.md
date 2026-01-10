```yaml
number: 9910
title: "Improve error message when `version` is missing from `pyproject.toml`"
type: issue
state: closed
author: edmorley
labels:
  - error messages
assignees: []
created_at: 2024-12-15T12:31:00Z
updated_at: 2024-12-15T15:27:44Z
url: https://github.com/astral-sh/uv/issues/9910
synced_at: 2026-01-10T04:36:21Z
```

# Improve error message when `version` is missing from `pyproject.toml`

---

_Issue opened by @edmorley on 2024-12-15 12:31_

The [pyproject.toml spec](https://packaging.python.org/en/latest/specifications/pyproject-toml/#declaring-project-metadata-the-project-table) says two fields under the `[project]` table are mandatory: `name` and `version` (unless `version` is declared as [dynamic](https://packaging.python.org/en/latest/specifications/pyproject-toml/#dynamic))

If I run `uv sync` with a `pyproject.toml` that's missing the `name` field:

```
[project]
version = "0.0.0"
requires-python = ">=3.13"
dependencies = []
```

...then I get a concise error message like:

```
$ uv sync
error: Failed to parse: `pyproject.toml`
  Caused by: `pyproject.toml` is using the `[project]` table, but the required `project.name` field is not set
  Caused by: TOML parse error at line 1, column 1
  |
1 | [project]
  | ^^^^^^^^^
missing field `name`
```

However, if instead the `version` field is missing, like so:


```
[project]
name = "testcase"
requires-python = ">=3.13"
dependencies = []
```

...then I get a Python stack trace rather than a TOML validation error:

```
$ uv sync
Using CPython 3.13.1 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Creating virtual environment at: .venv
  × Failed to build `testcase @ file:///Users/emorley/src/heroku-buildpack-python/spec/fixtures/uv_basic`
  ╰─▶ Build backend failed to determine requirements with `build_wheel()` (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 14, in <module>
          requires = get_requires_for_build({})
        File "/Users/emorley/.cache/uv/builds-v0/.tmp11hzLt/lib/python3.13/site-packages/setuptools/build_meta.py",
      line 334, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=[])
                 ~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/emorley/.cache/uv/builds-v0/.tmp11hzLt/lib/python3.13/site-packages/setuptools/build_meta.py",
      line 304, in _get_build_requires
          self.run_setup()
          ~~~~~~~~~~~~~~^^
        File "/Users/emorley/.cache/uv/builds-v0/.tmp11hzLt/lib/python3.13/site-packages/setuptools/build_meta.py",
      line 522, in run_setup
          super().run_setup(setup_script=setup_script)
          ~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/emorley/.cache/uv/builds-v0/.tmp11hzLt/lib/python3.13/site-packages/setuptools/build_meta.py",
      line 320, in run_setup
          exec(code, locals())
          ~~~~^^^^^^^^^^^^^^^^
        File "<string>", line 1, in <module>
          import sys
      
        File "/Users/emorley/.cache/uv/builds-v0/.tmp11hzLt/lib/python3.13/site-packages/setuptools/__init__.py",
      line 117, in setup
          return distutils.core.setup(**attrs)
                 ~~~~~~~~~~~~~~~~~~~~^^^^^^^^^
        File "/Users/emorley/.cache/uv/builds-v0/.tmp11hzLt/lib/python3.13/site-packages/setuptools/_distutils/core.py",
      line 157, in setup
          dist.parse_config_files()
          ~~~~~~~~~~~~~~~~~~~~~~~^^
        File "/Users/emorley/.cache/uv/builds-v0/.tmp11hzLt/lib/python3.13/site-packages/_virtualenv.py", line 20,
      in parse_config_files
          result = old_parse_config_files(self, *args, **kwargs)
        File "/Users/emorley/.cache/uv/builds-v0/.tmp11hzLt/lib/python3.13/site-packages/setuptools/dist.py", line
      648, in parse_config_files
          pyprojecttoml.apply_configuration(self, filename, ignore_option_errors)
          ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File
      "/Users/emorley/.cache/uv/builds-v0/.tmp11hzLt/lib/python3.13/site-packages/setuptools/config/pyprojecttoml.py",
      line 72, in apply_configuration
          config = read_configuration(filepath, True, ignore_option_errors, dist)
        File
      "/Users/emorley/.cache/uv/builds-v0/.tmp11hzLt/lib/python3.13/site-packages/setuptools/config/pyprojecttoml.py",
      line 140, in read_configuration
          validate(subset, filepath)
          ~~~~~~~~^^^^^^^^^^^^^^^^^^
        File
      "/Users/emorley/.cache/uv/builds-v0/.tmp11hzLt/lib/python3.13/site-packages/setuptools/config/pyprojecttoml.py",
      line 61, in validate
          raise ValueError(f"{error}\n{summary}") from None
      ValueError: invalid pyproject.toml config: `project`.
      configuration error: `project` must contain ['version'] properties
```

...which isn't as concise/readable as the `name` error message and comes across as though it's an unhandled internal error rather than being an intentional user facing error.

This `pyproject.toml` doesn't have a `[build-system]` section, so it seems as though uv shouldn't be getting setuptools to build it? (`version` isn't declared as [dynamic](https://packaging.python.org/en/latest/specifications/pyproject-toml/#dynamic) either, which is the only other reason I can think of why uv might be trying to build the project?)

This was using:

```
$ uv --version
uv 0.5.9 (Homebrew 2024-12-13)
```

---

_Comment by @edmorley on 2024-12-15 13:26_

Related: #7930

---

_Comment by @charliermarsh on 2024-12-15 13:42_

Yeah we should error early if version is neither present nor dynamic.

---

_Label `error messages` added by @charliermarsh on 2024-12-15 13:42_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-15 14:16_

---

_Closed by @charliermarsh on 2024-12-15 15:27_

---

_Closed by @charliermarsh on 2024-12-15 15:27_

---
