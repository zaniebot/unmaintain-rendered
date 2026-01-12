```yaml
number: 13734
title: Problem installing cuml on Windows
type: issue
state: closed
author: zoof
labels: []
assignees: []
created_at: 2025-05-30T16:35:42Z
updated_at: 2025-05-30T17:30:27Z
url: https://github.com/astral-sh/uv/issues/13734
synced_at: 2026-01-12T16:01:36Z
```

# Problem installing cuml on Windows

---

_@zoof_

### Summary

I'm trying to install cuml on a Windows 11 machine with cuda 12.9 installed but am getting an error message.  One of the messages is about setting a SPDX license expression and I've tried a couple but the error persists.  The command I'm running is "uv add cuml" and the error messages is:
```
Resolved 2 packages in 4ms
  × Failed to build `cuml==0.6.1.post1`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit code: 1)

      [stdout]
      running bdist_wheel
      running build
      installing to build\bdist.win-amd64\wheel
      running install

      [stderr]
      C:\Users\user\AppData\Local\uv\cache\builds-v0\.tmpGYDTmJ\Lib\site-packages\setuptools\dist.py:759:
      SetuptoolsDeprecationWarning: License classifiers are deprecated.
      !!

              ********************************************************************************
              Please consider removing the following classifiers in favor of a SPDX license expression:

              License :: OSI Approved :: Apache Software License

              See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license for details.
              ********************************************************************************

      !!
        self._finalize_license_expression()
      Traceback (most recent call last):
        File "<string>", line 11, in <module>
        File "C:\Users\user\AppData\Local\uv\cache\builds-v0\.tmpGYDTmJ\Lib\site-packages\setuptools\build_meta.py",
      line 432, in build_wheel
          return _build(['bdist_wheel'])
                 ^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\user\AppData\Local\uv\cache\builds-v0\.tmpGYDTmJ\Lib\site-packages\setuptools\build_meta.py",
      line 423, in _build
          return self._build_with_temp_dir(
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\user\AppData\Local\uv\cache\builds-v0\.tmpGYDTmJ\Lib\site-packages\setuptools\build_meta.py",
      line 404, in _build_with_temp_dir
          self.run_setup()
        File "C:\Users\user\AppData\Local\uv\cache\builds-v0\.tmpGYDTmJ\Lib\site-packages\setuptools\build_meta.py",
      line 512, in run_setup
          super().run_setup(setup_script=setup_script)
        File "C:\Users\user\AppData\Local\uv\cache\builds-v0\.tmpGYDTmJ\Lib\site-packages\setuptools\build_meta.py",
      line 317, in run_setup
          exec(code, locals())
        File "<string>", line 18, in <module>
        File "C:\Users\user\AppData\Local\uv\cache\builds-v0\.tmpGYDTmJ\Lib\site-packages\setuptools\__init__.py",
      line 115, in setup
          return distutils.core.setup(**attrs)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\user\AppData\Local\uv\cache\builds-v0\.tmpGYDTmJ\Lib\site-packages\setuptools\_distutils\core.py",
      line 186, in setup
          return run_commands(dist)
                 ^^^^^^^^^^^^^^^^^^
        File "C:\Users\user\AppData\Local\uv\cache\builds-v0\.tmpGYDTmJ\Lib\site-packages\setuptools\_distutils\core.py",
      line 202, in run_commands
          dist.run_commands()
        File "C:\Users\user\AppData\Local\uv\cache\builds-v0\.tmpGYDTmJ\Lib\site-packages\setuptools\_distutils\dist.py",
      line 1002, in run_commands
          self.run_command(cmd)
        File "C:\Users\user\AppData\Local\uv\cache\builds-v0\.tmpGYDTmJ\Lib\site-packages\setuptools\dist.py", line
      1102, in run_command
          super().run_command(command)
        File "C:\Users\user\AppData\Local\uv\cache\builds-v0\.tmpGYDTmJ\Lib\site-packages\setuptools\_distutils\dist.py",
      line 1021, in run_command
          cmd_obj.run()
        File
      "C:\Users\user\AppData\Local\uv\cache\builds-v0\.tmpGYDTmJ\Lib\site-packages\setuptools\command\bdist_wheel.py",
      line 405, in run
          self.run_command("install")
        File "C:\Users\user\AppData\Local\uv\cache\builds-v0\.tmpGYDTmJ\Lib\site-packages\setuptools\_distutils\cmd.py",
      line 357, in run_command
          self.distribution.run_command(command)
        File "C:\Users\user\AppData\Local\uv\cache\builds-v0\.tmpGYDTmJ\Lib\site-packages\setuptools\dist.py", line
      1102, in run_command
          super().run_command(command)
        File "C:\Users\user\AppData\Local\uv\cache\builds-v0\.tmpGYDTmJ\Lib\site-packages\setuptools\_distutils\dist.py",
      line 1021, in run_command
          cmd_obj.run()
        File "<string>", line 15, in run
      Exception: Please install cuml via the rapidsai conda channel. See https://rapids.ai/start.html for
      instructions.

      hint: This usually indicates a problem with the package or the build environment.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip
        locking and syncing.
```
If this is not a bug, can someone explain how to install it?

### Platform

Windows 11

### Version

uv 0.7.8 (0ddcc1905 2025-05-23)

### Python version

3.11

---

_Label `bug` added by @zoof on 2025-05-30 16:35_

---

_Label `bug` removed by @konstin on 2025-05-30 17:29_

---

_Comment by @konstin on 2025-05-30 17:30_

Please contact the cuml project for support for installing cuml, this is a choice by the cuml developers to use conda, not a bug in uv.

---

_Closed by @konstin on 2025-05-30 17:30_

---
