---
number: 2299
title: "Failed to add `paddle` dependency"
type: issue
state: closed
author: CrazyboyQCD
labels:
  - needs-mre
assignees: []
created_at: 2024-03-08T09:28:17Z
updated_at: 2024-03-09T14:13:36Z
url: https://github.com/astral-sh/uv/issues/2299
synced_at: 2026-01-10T01:23:15Z
---

# Failed to add `paddle` dependency

---

_Issue opened by @CrazyboyQCD on 2024-03-08 09:28_

**OS**: Windows 11
**Version**: 0.1.15
**Commd**: 
```rye add paddle```
**Error**: 
![image](https://github.com/astral-sh/uv/assets/53971641/07a0bf5e-17ef-4f8b-841a-d40673d65d99)
**The code snippet in it**:
![image](https://github.com/astral-sh/uv/assets/53971641/11f9de5c-554c-4548-ae04-5a41b39a7242)
**The directory of it**:
![image](https://github.com/astral-sh/uv/assets/53971641/9a1ac081-5abf-4c55-8ddb-344a27895999)

I got this error with `uv` bundled with latest `Rye` installed.

---

_Comment by @zanieb on 2024-03-08 15:18_

Hi! Can you share the full output of the command please?

---

_Label `needs-mre` added by @charliermarsh on 2024-03-08 15:22_

---

_Comment by @CrazyboyQCD on 2024-03-09 05:56_

> Hi! Can you share the full output of the command please?

@zanieb Here is full [output](https://github.com/astral-sh/uv/files/14545142/output.txt) .


---

_Comment by @charliermarsh on 2024-03-09 13:07_

This package also fails with pip using `--use-pep517`:

```
❯ pip install -e . --use-pep517
Obtaining file:///Users/crmarsh/Downloads/paddle-1.0.2
  Installing build dependencies ... done
  Checking if build backend supports build_editable ... done
  Getting requirements to build editable ... error
  error: subprocess-exited-with-error

  × Getting requirements to build editable did not run successfully.
  │ exit code: 1
  ╰─> [25 lines of output]
      Traceback (most recent call last):
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.0/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 353, in <module>
          main()
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.0/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 335, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.0/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 132, in get_requires_for_build_editable
          return hook(config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-zqvuxizl/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 448, in get_requires_for_build_editable
          return self.get_requires_for_build_wheel(config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-zqvuxizl/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=['wheel'])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-zqvuxizl/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
          self.run_setup()
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-zqvuxizl/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 487, in run_setup
          super().run_setup(setup_script=setup_script)
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-zqvuxizl/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 311, in run_setup
          exec(code, locals())
        File "<string>", line 3, in <module>
        File "/Users/crmarsh/Downloads/paddle-1.0.2/paddle/__init__.py", line 5, in <module>
          import common, dual, tight, data, prox
      ModuleNotFoundError: No module named 'common'
      [end of output]
```

---

_Comment by @charliermarsh on 2024-03-09 13:09_

Actually, this package just fails for me period:

```
❯ pip install paddle==1.0.2
Collecting paddle==1.0.2
  Downloading paddle-1.0.2.tar.gz (579 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 579.0/579.0 kB 6.8 MB/s eta 0:00:00
  Installing build dependencies ... done
  Getting requirements to build wheel ... error
  error: subprocess-exited-with-error

  × Getting requirements to build wheel did not run successfully.
  │ exit code: 1
  ╰─> [22 lines of output]
      Traceback (most recent call last):
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.0/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 353, in <module>
          main()
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.0/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 335, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.0/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 118, in get_requires_for_build_wheel
          return hook(config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-y9lthyxf/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=['wheel'])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-y9lthyxf/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
          self.run_setup()
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-y9lthyxf/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 487, in run_setup
          super().run_setup(setup_script=setup_script)
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-y9lthyxf/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 311, in run_setup
          exec(code, locals())
        File "<string>", line 3, in <module>
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-install-u0o9_bah/paddle_e146b46ed754421c896cef2bda401605/paddle/__init__.py", line 5, in <module>
          import common, dual, tight, data, prox
      ModuleNotFoundError: No module named 'common'
      [end of output]
```

Are you able to install it with `pip`? Otherwise, this should be raised with the package, not `uv`.

---

_Comment by @CrazyboyQCD on 2024-03-09 14:13_

> Are you able to install it with `pip`? Otherwise, this should be raised with the package, not `uv`.

Alright, I found that it is not an issue with uv itself but the package, I'll close this.


---

_Closed by @CrazyboyQCD on 2024-03-09 14:13_

---
