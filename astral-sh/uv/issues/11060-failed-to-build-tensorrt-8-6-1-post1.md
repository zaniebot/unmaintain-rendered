```yaml
number: 11060
title: "Failed to build `tensorrt==8.6.1.post1"
type: issue
state: closed
author: sh1man999
labels:
  - bug
assignees: []
created_at: 2025-01-29T12:59:25Z
updated_at: 2026-01-17T21:38:06Z
url: https://github.com/astral-sh/uv/issues/11060
synced_at: 2026-01-17T22:26:51Z
```

# Failed to build `tensorrt==8.6.1.post1

---

_@sh1man999_

### Summary

uv sync

```
[project]
name = "app"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10,<3.11"
dependencies = [
    "tensorrt-bindings==8.6.1",
    "tensorrt-libs==8.6.1",
    "tensorrt==8.6.1.post1"
]


[tool.uv.sources]

tensorrt = { index = "tensorrt"}
tensorrt-bindings = { index = "tensorrt"}
tensorrt-libs = { index = "tensorrt"}


[[tool.uv.index]]
name = "tensorrt"
url = "https://pypi.nvidia.com"
explicit = true

```


```

Built antlr4-python3-runtime==4.9.3
  × Failed to build `tensorrt==8.6.1.post1`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stdout]
      running bdist_wheel
      running build
      running build_py
      creating build/lib/tensorrt
      copying tensorrt/__init__.py -> build/lib/tensorrt
      running egg_info
      writing tensorrt.egg-info/PKG-INFO
      writing dependency_links to tensorrt.egg-info/dependency_links.txt
      writing requirements to tensorrt.egg-info/requires.txt
      writing top-level names to tensorrt.egg-info/top_level.txt
      reading manifest file 'tensorrt.egg-info/SOURCES.txt'
      adding license file 'LICENSE.txt'
      writing manifest file 'tensorrt.egg-info/SOURCES.txt'
      installing to build/bdist.linux-x86_64/wheel
      running install

      [stderr]
      /home/www-user/.cache/uv/builds-v0/.tmptah1W1/bin/python: No module named pip
      /home/www-user/.cache/uv/builds-v0/.tmptah1W1/lib/python3.11/site-packages/setuptools/_distutils/cmd.py:124: SetuptoolsDeprecationWarning: bdist_wheel.universal is deprecated
      !!

              ********************************************************************************
              With Python 2.7 end-of-life, support for building universal wheels
              (i.e., wheels that support both Python 2 and Python 3)
              is being obviated.
              Please discontinue using this option, or if you still need it,
              file an issue with pypa/setuptools describing your use case.

              By 2025-Aug-30, you need to update your project and remove deprecated calls
              or your builds will no longer be supported.
              ********************************************************************************

      !!
        self.finalize_options()
      /home/www-user/.cache/uv/builds-v0/.tmptah1W1/bin/python: No module named pip
      Traceback (most recent call last):
        File "<string>", line 11, in <module>
        File "/home/www-user/.cache/uv/builds-v0/.tmptah1W1/lib/python3.11/site-packages/setuptools/build_meta.py", line 435, in build_wheel
          return _build(['bdist_wheel'])
                 ^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/www-user/.cache/uv/builds-v0/.tmptah1W1/lib/python3.11/site-packages/setuptools/build_meta.py", line 426, in _build
          return self._build_with_temp_dir(
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/www-user/.cache/uv/builds-v0/.tmptah1W1/lib/python3.11/site-packages/setuptools/build_meta.py", line 407, in _build_with_temp_dir
          self.run_setup()
        File "/home/www-user/.cache/uv/builds-v0/.tmptah1W1/lib/python3.11/site-packages/setuptools/build_meta.py", line 522, in run_setup
          super().run_setup(setup_script=setup_script)
        File "/home/www-user/.cache/uv/builds-v0/.tmptah1W1/lib/python3.11/site-packages/setuptools/build_meta.py", line 320, in run_setup
          exec(code, locals())
        File "<string>", line 110, in <module>
        File "/home/www-user/.cache/uv/builds-v0/.tmptah1W1/lib/python3.11/site-packages/setuptools/__init__.py", line 117, in setup
          return distutils.core.setup(**attrs)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/www-user/.cache/uv/builds-v0/.tmptah1W1/lib/python3.11/site-packages/setuptools/_distutils/core.py", line 186, in setup
          return run_commands(dist)
                 ^^^^^^^^^^^^^^^^^^
        File "/home/www-user/.cache/uv/builds-v0/.tmptah1W1/lib/python3.11/site-packages/setuptools/_distutils/core.py", line 202, in run_commands
          dist.run_commands()
        File "/home/www-user/.cache/uv/builds-v0/.tmptah1W1/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 983, in run_commands
          self.run_command(cmd)
        File "/home/www-user/.cache/uv/builds-v0/.tmptah1W1/lib/python3.11/site-packages/setuptools/dist.py", line 999, in run_command
          super().run_command(command)
        File "/home/www-user/.cache/uv/builds-v0/.tmptah1W1/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 1002, in run_command
          cmd_obj.run()
        File "/home/www-user/.cache/uv/builds-v0/.tmptah1W1/lib/python3.11/site-packages/setuptools/command/bdist_wheel.py", line 414, in run
          self.run_command("install")
        File "/home/www-user/.cache/uv/builds-v0/.tmptah1W1/lib/python3.11/site-packages/setuptools/_distutils/cmd.py", line 339, in run_command
          self.distribution.run_command(command)
        File "/home/www-user/.cache/uv/builds-v0/.tmptah1W1/lib/python3.11/site-packages/setuptools/dist.py", line 999, in run_command
          super().run_command(command)
        File "/home/www-user/.cache/uv/builds-v0/.tmptah1W1/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 1002, in run_command
          cmd_obj.run()
        File "<string>", line 62, in run
        File "<string>", line 40, in run_pip_command
        File "/home/www-user/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu/lib/python3.11/subprocess.py", line 413, in check_call
          raise CalledProcessError(retcode, cmd)
      subprocess.CalledProcessError: Command '['/home/www-user/.cache/uv/builds-v0/.tmptah1W1/bin/python', '-m', 'pip', 'install', '--extra-index-url', 'https://pypi.nvidia.com', 'tensorrt_libs==8.6.1',
      'tensorrt_bindings==8.6.1']' returned non-zero exit status 1.
```

### Platform

Ubuntu 24.04

### Version

uv 0.5.24

### Python version

Python 3.11.11

---

_Label `bug` added by @sh1man999 on 2025-01-29 12:59_

---

_Comment by @charliermarsh on 2025-01-29 14:05_

It looks like `tensorrt` assumes that pip is available in the environment, or something? And calls out to it with subprocess?

https://github.com/NVIDIA/TensorRT/blob/97ff24489d0ea979c418c7a0847dfc14c8483846/python/packaging/frontend_sdist/setup.py#L38


---

_Comment by @charliermarsh on 2025-01-29 14:11_

Can you try running with `--verbose`?

---

_Comment by @zanieb on 2025-01-29 14:25_

Please see also, our build failure troubleshooting guide https://docs.astral.sh/uv/reference/troubleshooting/build-failures

---

_Comment by @lonless9 on 2025-02-01 20:32_

```
uv pip install pip setuptools
uv pip install tensorrt==8.6.1 --no-build-isolation-package tensorrt
```

---

_Closed by @charliermarsh on 2025-02-04 16:23_

---

_Comment by @khoover on 2026-01-17 21:38_

Small necro, I ran into the same thing and tried adding `extra-build-dependencies` and disabling build isolation without that working; it is a bit surprising the combo didn't cause `uv` to ensure those packages are pulled into the project environment, run the build, then drop them.

---
