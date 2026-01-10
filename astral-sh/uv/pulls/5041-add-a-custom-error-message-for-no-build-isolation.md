```yaml
number: 5041
title: "Add a custom error message for `--no-build-isolation` `torch` dependencies"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/torch
created_at: 2024-07-13T21:04:37Z
updated_at: 2024-07-13T21:12:27Z
url: https://github.com/astral-sh/uv/pull/5041
synced_at: 2026-01-10T13:42:52Z
```

# Add a custom error message for `--no-build-isolation` `torch` dependencies

---

_Pull request opened by @charliermarsh on 2024-07-13 21:04_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Closes https://github.com/astral-sh/uv/issues/5040.

## Test Plan

```
❯ cargo run pip install torch torch-scatter --no-cache
⠼ torch-scatter==2.1.2                                                                                                    error: Failed to download and build `torch-scatter==2.1.2`
  Caused by: Failed to build: `torch-scatter==2.1.2`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpuxrhWj/builds-v0/.tmp1OBLbw/lib/python3.12/site-packages/setuptools/build_meta.py", line 327, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpuxrhWj/builds-v0/.tmp1OBLbw/lib/python3.12/site-packages/setuptools/build_meta.py", line 297, in _get_build_requires
    self.run_setup()
  File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpuxrhWj/builds-v0/.tmp1OBLbw/lib/python3.12/site-packages/setuptools/build_meta.py", line 497, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpuxrhWj/builds-v0/.tmp1OBLbw/lib/python3.12/site-packages/setuptools/build_meta.py", line 313, in run_setup
    exec(code, locals())
  File "<string>", line 8, in <module>
ModuleNotFoundError: No module named 'torch'
---
  Caused by: This error likely indicates that torch-scatter==2.1.2 depends on torch, but doesn't declare it as a build dependency. If torch-scatter==2.1.2 is a first-party package, consider adding torch to its `build-system.requires`. Otherwise, `uv pip install torch` into the environment and re-run with `--no-build-isolation
```


---

_Renamed from "Add a custom error message for --no-build-isolation torch dependencies" to "Add a custom error message for `--no-build-isolation` `torch` dependencies" by @charliermarsh on 2024-07-13 21:04_

---

_Label `error messages` added by @charliermarsh on 2024-07-13 21:04_

---

_Merged by @charliermarsh on 2024-07-13 21:12_

---

_Closed by @charliermarsh on 2024-07-13 21:12_

---

_Branch deleted on 2024-07-13 21:12_

---
