---
number: 12626
title: "ValueError: Dev mode installations are unsupported with path rewrite"
type: issue
state: open
author: liquidcarbon
labels:
  - question
  - external
assignees: []
created_at: 2025-04-02T15:37:42Z
updated_at: 2025-04-14T19:14:32Z
url: https://github.com/astral-sh/uv/issues/12626
synced_at: 2026-01-10T01:25:22Z
---

# ValueError: Dev mode installations are unsupported with path rewrite

---

_Issue opened by @liquidcarbon on 2025-04-02 15:37_

### Question

On upgrading from 0.6.6 to 0.6.11 started getting errors coming from hatchling, referencing https://github.com/pfmoore/editables/issues/20

The section in pyproject.toml aims to package tests together with the package:

```
[tool.hatch.build.targets.wheel.sources]
"tests" = "mypkg/tests"
```

Output:
```
Resolved 102 packages in 280ms
  × Failed to build `pkg @ file:///data/code/pkg`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `hatchling.build.build_editable` failed (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 11, in <module>
        File "/home/a/.cache/uv/builds-v0/.tmpBZkDvI/lib/python3.12/site-packages/hatchling/build.py", line 83,
      in build_editable
          return os.path.basename(next(builder.build(directory=wheel_directory, versions=['editable'])))
                                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File
      "/home/a/.cache/uv/builds-v0/.tmpBZkDvI/lib/python3.12/site-packages/hatchling/builders/plugin/interface.py",
      line 155, in build
          artifact = version_api[version](directory, **build_data)
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/a/.cache/uv/builds-v0/.tmpBZkDvI/lib/python3.12/site-packages/hatchling/builders/wheel.py",
      line 496, in build_editable
          return self.build_editable_detection(directory, **build_data)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/a/.cache/uv/builds-v0/.tmpBZkDvI/lib/python3.12/site-packages/hatchling/builders/wheel.py",
      line 537, in build_editable_detection
          raise ValueError(message) from None
      ValueError: Dev mode installations are unsupported when any path rewrite in the `sources` option changes a
      prefix rather than removes it, see: https://github.com/pfmoore/editables/issues/20

      hint: This usually indicates a problem with the package or the build environment.
```


### Platform

all

### Version

0.6.11

---

_Label `question` added by @liquidcarbon on 2025-04-02 15:37_

---

_Comment by @charliermarsh on 2025-04-02 15:40_

Apologies, can you add a complete reproduction? Like a Git repo that we can clone and run? Or a clear set of instructions we can copy-paste? There isn't sufficient information here to help out.

---

_Label `question` removed by @charliermarsh on 2025-04-02 15:40_

---

_Label `needs-mre` added by @charliermarsh on 2025-04-02 15:40_

---

_Comment by @liquidcarbon on 2025-04-02 16:10_

different entry, same symptoms:

1) init project

```
uv init /home/a/pup/uv12626 -p /home/a/pup/.pixi/envs/default/bin/python --no-workspace
Initialized project `uv12626` at `/home/a/pup/uv12626`
```

2) add folders and files `cd uv12626; mkdir src tests; touch src/__init__.py tests/__init__.py`

3) edit pyproject.toml 

```
[project]
name = "uv12626"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "cowsay>=6.1",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build]
include = ["src/*", "/tests"]

[tool.hatch.build.targets.wheel.sources]
"tests" = "src/tests"
```

4) `uv add pytest --group dev`
```
Resolved 6 packages in 158ms
  × Failed to build `uv12626 @ file:///home/a/pup/uv12626`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `hatchling.build.build_editable` failed (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 11, in <module>
        File "/home/a/.cache/uv/builds-v0/.tmphJRlU0/lib/python3.12/site-packages/hatchling/build.py", line 83, in build_editable
          return os.path.basename(next(builder.build(directory=wheel_directory, versions=['editable'])))
                                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/a/.cache/uv/builds-v0/.tmphJRlU0/lib/python3.12/site-packages/hatchling/builders/plugin/interface.py", line 155, in build
          artifact = version_api[version](directory, **build_data)
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/a/.cache/uv/builds-v0/.tmphJRlU0/lib/python3.12/site-packages/hatchling/builders/wheel.py", line 496, in build_editable
          return self.build_editable_detection(directory, **build_data)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/a/.cache/uv/builds-v0/.tmphJRlU0/lib/python3.12/site-packages/hatchling/builders/wheel.py", line 537, in build_editable_detection
          raise ValueError(message) from None
      ValueError: Dev mode installations are unsupported when any path rewrite in the `sources` option changes a prefix rather than removes it, see: https://github.com/pfmoore/editables/issues/20

      hint: This usually indicates a problem with the package or the build environment.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```


**Unless group "dev" appears in the picture, it builds, adds, syncs without issues.**

---

_Comment by @liquidcarbon on 2025-04-14 12:36_

Have you had a chance to take a look?  Still happening in 0.6.14, must sync with `--no-editable`, will need to build eventually

---

_Comment by @konstin on 2025-04-14 12:40_

This looks like a problem with your hatchling configuration, not uv (the problem should also occur with `pip install -e .`). Consider not including the tests in the wheel build, if the problem persists you might want to raise it with hatchling instead.

---

_Label `needs-mre` removed by @konstin on 2025-04-14 12:40_

---

_Label `question` added by @konstin on 2025-04-14 12:40_

---

_Referenced in [pypa/hatch#1956](../../pypa/hatch/issues/1956.md) on 2025-04-14 12:49_

---

_Comment by @liquidcarbon on 2025-04-14 12:49_

@konstin thanks.  I've worked out that approach to packaging on uv 0.6.6 (did not keep track of hatch versions) - something seems to have changed in recent versions.  I'm not sure why renaming paths causes problems.

https://github.com/pypa/hatch/issues/1956

---

_Label `external` added by @konstin on 2025-04-14 14:15_

---

_Comment by @liquidcarbon on 2025-04-14 19:12_

I think the emergence makes sense in light of https://github.com/astral-sh/uv/pull/12137 (not this exact PR but altogether the changes around `.pth` files?)

Previously, the package would appear in `.venv/lib/python3.1x/site-packages/` as if it was pip installed there; now there's only the `.pth` file pointing to the source dir.  Moving folders around would probably not work if you plan to import as editables?



---
