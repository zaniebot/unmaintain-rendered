```yaml
number: 6525
title: "uv run doesn't work without __init__.py"
type: issue
state: closed
author: AndreuCodina
labels:
  - enhancement
assignees: []
created_at: 2024-08-23T15:28:23Z
updated_at: 2024-08-28T18:06:08Z
url: https://github.com/astral-sh/uv/issues/6525
synced_at: 2026-01-12T15:59:05Z
```

# uv run doesn't work without __init__.py

---

_@AndreuCodina_

The `__init__.py` file is optional, but if you download the repository https://github.com/astral-sh/uv-fastapi-example (a fresh copy, don't re-use the repository) and remove the `app/__init__.py` file, when you run:

```bash
uv run fastapi dev app/main.py
```

then you get this error:

```
Using Python 3.12.1 interpreter at: /Library/Frameworks/Python.framework/Versions/3.12/bin/python3
Creating virtualenv at: .venv
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: app @ file:///Users/andreu/Git/Andreu/uv-fastapi-example
  Caused by: Build backend failed to build wheel through `build_editable()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 11, in <module>
  File "/Users/.../Library/Caches/uv/builds-v0/.tmpfYT0UH/lib/python3.12/site-packages/hatchling/build.py", line 83, in build_editable
    return os.path.basename(next(builder.build(directory=wheel_directory, versions=['editable'])))
                            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/.../Library/Caches/uv/builds-v0/.tmpfYT0UH/lib/python3.12/site-packages/hatchling/builders/plugin/interface.py", line 155, in build
    artifact = version_api[version](directory, **build_data)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/.../Library/Caches/uv/builds-v0/.tmpfYT0UH/lib/python3.12/site-packages/hatchling/builders/wheel.py", line 494, in build_editable
    return self.build_editable_detection(directory, **build_data)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/.../Library/Caches/uv/builds-v0/.tmpfYT0UH/lib/python3.12/site-packages/hatchling/builders/wheel.py", line 505, in build_editable_detection
    for included_file in self.recurse_selected_project_files():
  File "/Users/.../Library/Caches/uv/builds-v0/.tmpfYT0UH/lib/python3.12/site-packages/hatchling/builders/plugin/interface.py", line 180, in recurse_selected_project_files
    if self.config.only_include:
       ^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/.../Library/Caches/uv/builds-v0/.tmpfYT0UH/lib/python3.12/site-packages/hatchling/builders/config.py", line 806, in only_include
    only_include = only_include_config.get('only-include', self.default_only_include()) or self.packages
                                                           ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/.../Library/Caches/uv/builds-v0/.tmpfYT0UH/lib/python3.12/site-packages/hatchling/builders/wheel.py", line 260, in default_only_include
    return self.default_file_selection_options.only_include
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/functools.py", line 995, in __get__
    val = self.func(instance)
          ^^^^^^^^^^^^^^^^^^^
  File "/Users/.../Library/Caches/uv/builds-v0/.tmpfYT0UH/lib/python3.12/site-packages/hatchling/builders/wheel.py", line 248, in default_file_selection_options
    raise ValueError(message)
ValueError: Unable to determine which files to ship inside the wheel using the following heuristics: https://hatch.pypa.io/latest/plugins/builder/wheel/#default-file-selection

The most likely cause of this is that there is no directory that matches the name of your project (app).

At least one file selection option must be defined in the `tool.hatch.build.targets.wheel` table, see: https://hatch.pypa.io/latest/config/build/

As an example, if you intend to ship a directory named `foo` that resides within a `src` directory located at the root of your project, you can define the following:

[tool.hatch.build.targets.wheel]
packages = ["src/foo"]
```

--------

**uv version:** 0.3.2

---

_Comment by @charliermarsh on 2024-08-25 02:32_

I think describing `__init__.py` as optional is a bit of a misnomer. Python code sometimes works without an `__init__.py` due to namespace packages, but that's often "by accident". Regardless, this would be fixed by #6585.

---

_Label `enhancement` added by @charliermarsh on 2024-08-25 02:32_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-26 14:44_

---

_Comment by @charliermarsh on 2024-08-28 18:06_

This should be fixed in [v0.4.0](https://github.com/astral-sh/uv/releases/tag/0.4.0). Projects that omit `[build-system]` will no longer be built and installed by default. You can also set `tool.uv.package = false` or `tool.uv.package = false` to control the behavior explicitly.

---

_Closed by @charliermarsh on 2024-08-28 18:06_

---
