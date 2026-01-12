```yaml
number: 15533
title: Add logging to the uv build backend
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - build-backend
assignees: []
merged: true
base: main
head: konsti/build-backend-logging
created_at: 2025-08-26T14:01:55Z
updated_at: 2025-08-27T07:14:02Z
url: https://github.com/astral-sh/uv/pull/15533
synced_at: 2026-01-12T16:11:48Z
```

# Add logging to the uv build backend

---

_@konstin_

Add support for `RUST_LOG` to the uv build backend. While we were previously using logging statements in the uv build backend, they could only be shown when when using the direct build fast path through uv, as there was no tracing subscriber to write log messages out. This means no debug logging when using the build backend through pip, `python -m build`, an incompatible version of uv, or any other build frontend; No option to figure why includes and excludes behave the way they do.

This PR closes this gap by adding a tracing subscriber. The only option to enable it is `RUST_LOG`, as we don't have a CLI. The formatting style is the same as for uv, and color is also support in the same way, albeit only through anstream's support for TTYs and environment variables. We recommend only `RUST_LOG=uv=debug` and `RUST_LOG=uv=verbose` in the docs, but this can be used to debug into crates such as `glob`, too.

<img width="1008" height="325" alt="image" src="https://github.com/user-attachments/assets/d33df219-750b-46a2-b3b4-8895aa137ab9" />

**Before**

```
$ pip wheel . -v [...]
Looking in links: /home/konsti/projects/uv/target/wheels/
Processing /home/konsti/projects/uv/scripts/packages/built-by-uv
  Running command pip subprocess to install build dependencies
  Looking in links: /home/konsti/projects/uv/target/wheels/
  Processing /home/konsti/projects/uv/target/wheels/uv_build-0.8.13-py3-none-manylinux_2_39_x86_64.whl
  Installing collected packages: uv_build
  Successfully installed uv_build-0.8.13
  Installing build dependencies ... done
  Running command Getting requirements to build wheel
  Getting requirements to build wheel ... done
  Running command Preparing metadata (pyproject.toml)
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: built-by-uv
  Running command Building wheel for built-by-uv (pyproject.toml)
  Error: Unsupported glob expression in: `tool.uv.build-backend.*-exclude`

  Caused by:
      Invalid character `!` at position 10 in glob: `**/build-*!$§%!½¼²¼³¬!§%$§%.h`. hint: Characters can be escaped with a backslash
  error: subprocess-exited-with-error

  × Building wheel for built-by-uv (pyproject.toml) did not run successfully.
  │ exit code: 1
  ╰─> See above for output.

  note: This error originates from a subprocess, and is likely not a problem with pip.
  full command: /usr/bin/python3 /usr/lib/python3/dist-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py build_wheel /tmp/tmpow1illc9
  cwd: /home/konsti/projects/uv/scripts/packages/built-by-uv
  Building wheel for built-by-uv (pyproject.toml) ... error
  ERROR: Failed building wheel for built-by-uv
Failed to build built-by-uv
ERROR: Failed to build one or more wheels
```

**After**

```
$ RUST_LOG=uv=debug pip wheel . -v [...]
Looking in links: /home/konsti/projects/uv/target/wheels/
Processing /home/konsti/projects/uv/scripts/packages/built-by-uv
  Running command pip subprocess to install build dependencies
  Looking in links: /home/konsti/projects/uv/target/wheels/
  Processing /home/konsti/projects/uv/target/wheels/uv_build-0.8.13-py3-none-manylinux_2_39_x86_64.whl
  Installing collected packages: uv_build
  Successfully installed uv_build-0.8.13
  Installing build dependencies ... done
  Running command Getting requirements to build wheel
  Getting requirements to build wheel ... done
  Running command Preparing metadata (pyproject.toml)
  DEBUG Writing metadata files to /tmp/pip-modern-metadata-l_kh78cj
  DEBUG Found PEP 639 license declarations, using METADATA 2.4
  DEBUG License files match: `LICENSE-APACHE`
  DEBUG License files match: `LICENSE-MIT`
  DEBUG License files match: `third-party-licenses/PEP-401.txt`
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: built-by-uv
  Running command Building wheel for built-by-uv (pyproject.toml)
  DEBUG Checking metadata directory /tmp/pip-modern-metadata-l_kh78cj/built_by_uv-0.1.0.dist-info
  DEBUG Found PEP 639 license declarations, using METADATA 2.4
  DEBUG License files match: `LICENSE-APACHE`
  DEBUG License files match: `LICENSE-MIT`
  DEBUG License files match: `third-party-licenses/PEP-401.txt`
  DEBUG Writing wheel at /tmp/pip-wheel-bu6to9i7/built_by_uv-0.1.0-py3-none-any.whl
  DEBUG Wheel excludes: ["__pycache__", "*.pyc", "*.pyo", "build-*!$§%!½¼²¼³¬!§%$§%.h", "/src/built_by_uv/not-packaged.txt"]
  Error: Unsupported glob expression in: `tool.uv.build-backend.*-exclude`

  Caused by:
      Invalid character `!` at position 10 in glob: `**/build-*!$§%!½¼²¼³¬!§%$§%.h`. hint: Characters can be escaped with a backslash
  error: subprocess-exited-with-error

  × Building wheel for built-by-uv (pyproject.toml) did not run successfully.
  │ exit code: 1
  ╰─> See above for output.

  note: This error originates from a subprocess, and is likely not a problem with pip.
  full command: /usr/bin/python3 /usr/lib/python3/dist-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py build_wheel /tmp/tmpjrxou13a
  cwd: /home/konsti/projects/uv/scripts/packages/built-by-uv
  Building wheel for built-by-uv (pyproject.toml) ... error
  ERROR: Failed building wheel for built-by-uv
Failed to build built-by-uv
ERROR: Failed to build one or more wheels
```

(There is no color in the above uv log statements, as pip doesn't register as a TTY)

Fixes #12723


---

_Review requested from @BurntSushi by @konstin on 2025-08-26 14:01_

---

_Review requested from @zanieb by @konstin on 2025-08-26 14:01_

---

_Label `enhancement` added by @konstin on 2025-08-26 14:01_

---

_Label `build-backend` added by @konstin on 2025-08-26 14:01_

---

_@zanieb approved on 2025-08-26 14:41_

---

_@BurntSushi approved on 2025-08-26 17:55_

---

_Merged by @konstin on 2025-08-27 07:14_

---

_Closed by @konstin on 2025-08-27 07:14_

---

_Branch deleted on 2025-08-27 07:14_

---
