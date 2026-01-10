---
number: 6933
title: "`uv add` confuses dependency versions when installing multiple libraries"
type: issue
state: closed
author: notypecheck
labels:
  - bug
assignees: []
created_at: 2024-09-02T10:03:57Z
updated_at: 2024-09-02T18:21:26Z
url: https://github.com/astral-sh/uv/issues/6933
synced_at: 2026-01-10T01:24:08Z
---

# `uv add` confuses dependency versions when installing multiple libraries

---

_Issue opened by @notypecheck on 2024-09-02 10:03_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
I've been migrating a project from pdm to uv and faced an error when installed dev libraries.

## Environment info
uv version:
```sh
$ uv --version
uv 0.4.2 (736ccb950 2024-09-01)
```
OS: Windows, MacOS

## Steps to reproduce
```sh
uv init
```
```sh
uv add --dev mypy ruff radon vulture pytest coverage factory-boy sqlalchemy-pytest types-python-dateutil jinja2 types-click deptry types-openpyxl types-aiobotocore-s3
```
```sh
Resolved 32 packages in 1.64s
  × No solution found when resolving dependencies:
  ╰─▶ Because only ruff<=0.6.3 is available and test-uv:dev depends on ruff>=2.9.0.20240821, we can conclude
      that test-uv:dev's requirements are unsatisfiable.
      And because your project depends on test-uv:dev, we can conclude that your project's requirements are
      unsatisfiable.
  help: If this is intentional, run `uv add --frozen` to skip the lock and sync steps.
```
What interesting is that in lockfile correct version of ruff is used (0.6.3),

In this particular case version `2.9.0.20240821` comes from `types-python-dateutil`


---

_Renamed from "`uv add` fails when used to install multiple libraries" to "`uv add --dev` fails when used to install multiple libraries" by @notypecheck on 2024-09-02 10:04_

---

_Renamed from "`uv add --dev` fails when used to install multiple libraries" to "`uv add --dev` confuses dependency versions when installing multiple libraries" by @notypecheck on 2024-09-02 10:05_

---

_Renamed from "`uv add --dev` confuses dependency versions when installing multiple libraries" to "`uv add` confuses dependency versions when installing multiple libraries" by @notypecheck on 2024-09-02 10:06_

---

_Comment by @erob-archim on 2024-09-02 10:42_

Adding another example in case it's helpful:

```
>>> uv add torchgeo torch numpy geopandas result shapely pyproj opencv-python-headless rasterio rustedpy-maybe loguru adbc-driver-postgresql scipy pyarrow
Resolved 95 packages in 0.90ms
  × No solution found when resolving dependencies for split (python_full_version < '3.11' and platform_system == 'Darwin' and sys_platform == 'linux'):
  ╰─▶ Because only loguru<=0.7.2 is available and your project depends on loguru>=4.10.0.84, we can conclude that your project's requirements are unsatisfiable.
  help: If this is intentional, run `uv add --frozen` to skip the lock and sync steps.
>>> uv add torchgeo torch numpy geopandas result shapely pyproj opencv-python-headless rasterio rustedpy-maybe adbc-driver-postgresql scipy pyarrow
Resolved 94 packages in 354ms
  × No solution found when resolving dependencies for split (python_full_version < '3.11' and platform_system == 'Darwin' and sys_platform == 'linux'):
  ╰─▶ Because only numpy<=2.1.0 is available and your project depends on numpy>=4.10.0.84, we can conclude that your project's requirements are unsatisfiable.
  help: If this is intentional, run `uv add --frozen` to skip the lock and sync steps.
  ```

Interesting that it's saying the same incorrect version number for both numpy and loguru

---

_Comment by @charliermarsh on 2024-09-02 17:19_

Thanks, I'll look into this ASAP.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-02 17:19_

---

_Label `bug` added by @charliermarsh on 2024-09-02 17:20_

---

_Comment by @charliermarsh on 2024-09-02 17:43_

It's a bug from #6388. I'll either fix and re-release, or revert.

---

_Referenced in [astral-sh/uv#6939](../../astral-sh/uv/pulls/6939.md) on 2024-09-02 17:57_

---

_Closed by @charliermarsh on 2024-09-02 18:21_

---

_Closed by @charliermarsh on 2024-09-02 18:21_

---
