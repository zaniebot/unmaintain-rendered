```yaml
number: 9991
title: "build backend: `find_uv_bin` fails with pip"
type: issue
state: closed
author: cthoyt
labels:
  - bug
  - preview
assignees: []
created_at: 2024-12-18T09:46:29Z
updated_at: 2024-12-20T09:15:25Z
url: https://github.com/astral-sh/uv/issues/9991
synced_at: 2026-01-10T04:36:21Z
```

# build backend: `find_uv_bin` fails with pip

---

_Issue opened by @cthoyt on 2024-12-18 09:46_

pip has an issue installing a project using the uv backend, both when installing in editable mode and not. My system uses the following:

- uv 0.5.10 (37b11ddb2 2024-12-17). 
- Python 3.12.7
- pip 24.3.1

This can be reproduced with the following:

```console
$ uv --preview init xyz --build-backend uv
Initialized project `xyz` at `/Users/cthoyt/Desktop/xyz`

$ cd xyz
```


Here's the error from regular install. Note that `UV_PREVIEW=1` is required here, otherwise the uv package doesn't have the right functions exposed, and unlike when using uv directly, we can't pass `--preview`.

```console
$ UV_PREVIEW=1 python3 -m pip install .
Processing /Users/cthoyt/Desktop/xyz
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... error
  error: subprocess-exited-with-error
  
  × Preparing metadata (pyproject.toml) did not run successfully.
  │ exit code: 1
  ╰─> [18 lines of output]
      Traceback (most recent call last):
        File "/Users/cthoyt/.virtualenvs/biopragmatics/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 353, in <module>
          main()
        File "/Users/cthoyt/.virtualenvs/biopragmatics/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 335, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/cthoyt/.virtualenvs/biopragmatics/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 149, in prepare_metadata_for_build_wheel
          return hook(metadata_directory, config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/6k/r2gmmc213vl6h7bv20x624xw0000gn/T/pip-build-env-8hdutcpq/overlay/lib/python3.12/site-packages/uv/_build_backend.py", line 84, in prepare_metadata_for_build_wheel
          return call(args, config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/6k/r2gmmc213vl6h7bv20x624xw0000gn/T/pip-build-env-8hdutcpq/overlay/lib/python3.12/site-packages/uv/_build_backend.py", line 36, in call
          result = subprocess.run([find_uv_bin()] + args, stdout=subprocess.PIPE)
                                   ^^^^^^^^^^^^^
        File "/private/var/folders/6k/r2gmmc213vl6h7bv20x624xw0000gn/T/pip-build-env-8hdutcpq/overlay/lib/python3.12/site-packages/uv/_find_uv.py", line 36, in find_uv_bin
          raise FileNotFoundError(path)
      FileNotFoundError: /Users/cthoyt/Library/Python/3.12/bin/uv
      [end of output]
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
error: metadata-generation-failed

× Encountered error while generating package metadata.
╰─> See above for output.

note: This is an issue with the package mentioned above, not pip.
hint: See above for details.
```

Here's the error from editable install

```console
$ UV_PREVIEW=1 python3 -m pip install -e .
Obtaining file:///Users/cthoyt/Desktop/xyz
  Installing build dependencies ... done
  Checking if build backend supports build_editable ... done
  Getting requirements to build editable ... done
  Preparing editable metadata (pyproject.toml) ... error
  error: subprocess-exited-with-error
  
  × Preparing editable metadata (pyproject.toml) did not run successfully.
  │ exit code: 1
  ╰─> [18 lines of output]
      Traceback (most recent call last):
        File "/Users/cthoyt/.virtualenvs/biopragmatics/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 353, in <module>
          main()
        File "/Users/cthoyt/.virtualenvs/biopragmatics/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 335, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/cthoyt/.virtualenvs/biopragmatics/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 181, in prepare_metadata_for_build_editable
          return hook(metadata_directory, config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/6k/r2gmmc213vl6h7bv20x624xw0000gn/T/pip-build-env-3lz54y_v/overlay/lib/python3.12/site-packages/uv/_build_backend.py", line 110, in prepare_metadata_for_build_editable
          return call(args, config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/6k/r2gmmc213vl6h7bv20x624xw0000gn/T/pip-build-env-3lz54y_v/overlay/lib/python3.12/site-packages/uv/_build_backend.py", line 36, in call
          result = subprocess.run([find_uv_bin()] + args, stdout=subprocess.PIPE)
                                   ^^^^^^^^^^^^^
        File "/private/var/folders/6k/r2gmmc213vl6h7bv20x624xw0000gn/T/pip-build-env-3lz54y_v/overlay/lib/python3.12/site-packages/uv/_find_uv.py", line 36, in find_uv_bin
          raise FileNotFoundError(path)
      FileNotFoundError: /Users/cthoyt/Library/Python/3.12/bin/uv
      [end of output]
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
error: metadata-generation-failed

× Encountered error while generating package metadata.
╰─> See above for output.

note: This is an issue with the package mentioned above, not pip.
hint: See above for details.

---

_Assigned to @konstin by @charliermarsh on 2024-12-18 17:23_

---

_Label `bug` added by @charliermarsh on 2024-12-18 17:23_

---

_Label `preview` added by @charliermarsh on 2024-12-18 17:23_

---

_Renamed from "build backend: issue locating uv executable when installing with pip" to "build backend: `find_uv_bin` fails with pip" by @konstin on 2024-12-18 19:14_

---

_Closed by @konstin on 2024-12-20 09:15_

---
