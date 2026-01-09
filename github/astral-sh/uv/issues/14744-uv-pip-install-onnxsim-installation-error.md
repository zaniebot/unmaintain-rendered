---
number: 14744
title: "`uv pip install onnxsim` installation error"
type: issue
state: open
author: kayoonkim
labels:
  - bug
assignees: []
created_at: 2025-07-19T12:36:22Z
updated_at: 2025-07-19T14:10:31Z
url: https://github.com/astral-sh/uv/issues/14744
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv pip install onnxsim` installation error

---

_Issue opened by @kayoonkim on 2025-07-19 12:36_

### Summary

Hi, I'm encountering an issue while installing `onnxsim`.
Here's the complete error message:

`× Failed to build `onnxsim==0.4.36`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stdout]
      running bdist_wheel
      running build
      running build_py
      running create_version
      copying onnxsim/__main__.py -> build/lib.linux-aarch64-cpython-312/onnxsim
      copying onnxsim/onnx_simplifier.py -> build/lib.linux-aarch64-cpython-312/onnxsim
      copying onnxsim/version.py -> build/lib.linux-aarch64-cpython-312/onnxsim
      copying onnxsim/model_checking.py -> build/lib.linux-aarch64-cpython-312/onnxsim
      copying onnxsim/model_info.py -> build/lib.linux-aarch64-cpython-312/onnxsim
      copying onnxsim/__init__.py -> build/lib.linux-aarch64-cpython-312/onnxsim
      running egg_info
      writing onnxsim.egg-info/PKG-INFO
      writing dependency_links to onnxsim.egg-info/dependency_links.txt
      writing entry points to onnxsim.egg-info/entry_points.txt
      writing requirements to onnxsim.egg-info/requires.txt
      writing top-level names to onnxsim.egg-info/top_level.txt
      reading manifest file 'onnxsim.egg-info/SOURCES.txt'
      reading manifest template 'MANIFEST.in'
      adding license file 'LICENSE'
      writing manifest file 'onnxsim.egg-info/SOURCES.txt'
      copying onnxsim/cpp2py_export.cc -> build/lib.linux-aarch64-cpython-312/onnxsim
      copying onnxsim/cxxopts.hpp -> build/lib.linux-aarch64-cpython-312/onnxsim
      copying onnxsim/onnxsim.cpp -> build/lib.linux-aarch64-cpython-312/onnxsim
      copying onnxsim/onnxsim.h -> build/lib.linux-aarch64-cpython-312/onnxsim
      copying onnxsim/bin/onnxsim_bin.cpp -> build/lib.linux-aarch64-cpython-312/onnxsim/bin
      copying onnxsim/bin/onnxsim_option.cpp -> build/lib.linux-aarch64-cpython-312/onnxsim/bin
      copying onnxsim/bin/onnxsim_option.h -> build/lib.linux-aarch64-cpython-312/onnxsim/bin
      running build_ext
      running cmake_build
      -- Configuring incomplete, errors occurred!
      Run command ['/home/ubuntu/my_folder_name/.venv/bin/cmake',
      '-DPython_INCLUDE_DIR=/home/ubuntu/.local/share/uv/python/cpython-3.12.7-linux-aarch64-gnu/include/python3.12',
      '-DPython_EXECUTABLE=/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/bin/python',
      '-DPYTHON_EXECUTABLE=/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/bin/python',
      '-DBUILD_ONNX_PYTHON=OFF', '-DONNXSIM_PYTHON=ON', '-DONNXSIM_BUILTIN_ORT=OFF',
      '-DONNX_USE_LITE_PROTO=OFF', '-DCMAKE_EXPORT_COMPILE_COMMANDS=ON',
      '-DONNX_NAMESPACE=onnx', '-DPY_EXT_SUFFIX=.cpython-312-aarch64-linux-gnu.so',
      '-DONNX_OPT_USE_SYSTEM_PROTOBUF=OFF', '-DCMAKE_BUILD_TYPE=Release', '-DONNX_ML=1',
      '/home/ubuntu/.cache/uv/sdists-v9/pypi/onnxsim/0.4.36/I-UFDVCqugrMo4L-jGXwF/src']

      [stderr]
      <string>:28: DeprecationWarning: Use shutil.which instead of find_executable
      fatal: invalid gitfile format: /home/ubuntu/.cache/uv/sdists-v9/.git
      fatal: invalid gitfile format: /home/ubuntu/.cache/uv/sdists-v9/.git
      /home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/dist.py:759:
      SetuptoolsDeprecationWarning: License classifiers are deprecated.
      !!

              ********************************************************************************
              Please consider removing the following classifiers in favor of a SPDX license expression:

              License :: OSI Approved :: Apache Software License

              See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license for details.
              ********************************************************************************

      !!
        self._finalize_license_expression()
      warning: no files found matching '*.c' under directory 'onnxsim'
      warning: no files found matching '*.proto' under directory 'onnxsim'
      warning: no previously-included files matching '*' found under directory 'third_party/onnxruntime'
      warning: no previously-included files matching '*' found under directory 'third_party/onnx-optimizer/build'
      warning: no previously-included files matching '*' found under directory 'third_party/onnx/build'
      warning: no previously-included files matching '*' found under directory 'third_party/onnx/onnx/backend'
      /home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/command/build_py.py:212:
      _Warning: Package 'onnxsim.bin' is absent from the `packages` configuration.
      !!

              ********************************************************************************
              ############################
              # Package would be ignored #
              ############################
              Python recognizes 'onnxsim.bin' as an importable package[^1],
              but it is absent from setuptools' `packages` configuration.

              This leads to an ambiguous overall configuration. If you want to distribute this
              package, please make sure that 'onnxsim.bin' is explicitly added
              to the `packages` configuration field.

              Alternatively, you can also rely on setuptools' discovery methods
              (for example by using `find_namespace_packages(...)`/`find_namespace:`
              instead of `find_packages(...)`/`find:`).

              You can read more about "package discovery" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/package_discovery.html

              If you don't want 'onnxsim.bin' to be distributed and are
              already explicitly excluding 'onnxsim.bin' via
              `find_namespace_packages(...)/find_namespace` or `find_packages(...)/find`,
              you can try to use `exclude_package_data`, or `include-package-data=False` in
              combination with a more fine grained `package-data` configuration.

              You can read more about "package data files" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/datafiles.html


              [^1]: For Python, any directory (with suitable naming) can be imported,
                    even if it does not contain any `.py` files.
                    On the other hand, currently there is no concept of package data
                    directory, all directories are treated like packages.
              ********************************************************************************

      !!
        check.warn(importable)
      CMake Error: CMake was unable to find a build program corresponding to "Unix Makefiles".  CMAKE_MAKE_PROGRAM
      is not set.  You probably need to select a different build tool.
      CMake Error: CMAKE_CXX_COMPILER not set, after EnableLanguage
      Traceback (most recent call last):
        File "<string>", line 11, in <module>
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/build_meta.py",
      line 432, in build_wheel
          return _build(['bdist_wheel'])
                 ^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/build_meta.py",
      line 423, in _build
          return self._build_with_temp_dir(
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/build_meta.py",
      line 404, in _build_with_temp_dir
          self.run_setup()
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/build_meta.py",
      line 512, in run_setup
          super().run_setup(setup_script=setup_script)
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/build_meta.py",
      line 317, in run_setup
          exec(code, locals())
        File "<string>", line 279, in <module>
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/__init__.py",
      line 115, in setup
          return distutils.core.setup(**attrs)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/_distutils/core.py",
      line 186, in setup
          return run_commands(dist)
                 ^^^^^^^^^^^^^^^^^^
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/_distutils/core.py",
      line 202, in run_commands
          dist.run_commands()
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/_distutils/dist.py",
      line 1002, in run_commands
          self.run_command(cmd)
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/dist.py", line
      1102, in run_command
          super().run_command(command)
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/_distutils/dist.py",
      line 1021, in run_command
          cmd_obj.run()
        File
      "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/command/bdist_wheel.py",
      line 370, in run
          self.run_command("build")
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/_distutils/cmd.py",
      line 357, in run_command
          self.distribution.run_command(command)
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/dist.py", line
      1102, in run_command
          super().run_command(command)
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/_distutils/dist.py",
      line 1021, in run_command
          cmd_obj.run()
        File
      "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/_distutils/command/build.py",
      line 135, in run
          self.run_command(cmd_name)
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/_distutils/cmd.py",
      line 357, in run_command
          self.distribution.run_command(command)
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/dist.py", line
      1102, in run_command
          super().run_command(command)
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/_distutils/dist.py",
      line 1021, in run_command
          cmd_obj.run()
        File "<string>", line 220, in run
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/_distutils/cmd.py",
      line 357, in run_command
          self.distribution.run_command(command)
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/dist.py", line
      1102, in run_command
          super().run_command(command)
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/lib/python3.12/site-packages/setuptools/_distutils/dist.py",
      line 1021, in run_command
          cmd_obj.run()
        File "<string>", line 194, in run
        File "/home/ubuntu/.local/share/uv/python/cpython-3.12.7-linux-aarch64-gnu/lib/python3.12/subprocess.py",
      line 413, in check_call
          raise CalledProcessError(retcode, cmd)
      subprocess.CalledProcessError: Command '['/home/ubuntu/my_folder_name/.venv/bin/cmake',
      '-DPython_INCLUDE_DIR=/home/ubuntu/.local/share/uv/python/cpython-3.12.7-linux-aarch64-gnu/include/python3.12',
      '-DPython_EXECUTABLE=/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/bin/python',
      '-DPYTHON_EXECUTABLE=/home/ubuntu/.cache/uv/builds-v0/.tmpPNxw58/bin/python',
      '-DBUILD_ONNX_PYTHON=OFF', '-DONNXSIM_PYTHON=ON', '-DONNXSIM_BUILTIN_ORT=OFF',
      '-DONNX_USE_LITE_PROTO=OFF', '-DCMAKE_EXPORT_COMPILE_COMMANDS=ON',
      '-DONNX_NAMESPACE=onnx', '-DPY_EXT_SUFFIX=.cpython-312-aarch64-linux-gnu.so',
      '-DONNX_OPT_USE_SYSTEM_PROTOBUF=OFF', '-DCMAKE_BUILD_TYPE=Release', '-DONNX_ML=1',
      '/home/ubuntu/.cache/uv/sdists-v9/pypi/onnxsim/0.4.36/I-UFDVCqugrMo4L-jGXwF/src']' returned non-zero exit
      status 1.

      hint: This usually indicates a problem with the package or the build environment.`


I tried installing a lower version of Python (3.12.7) because I read a thread where someone successfully installed it with that version (https://github.com/daquexian/onnx-simplifier/issues/334). I also used an earlier version of `onnxsim`. I reinstalled the virtual environment, but it still isn't working. Has anyone else encountered the same issue? Or could you tell me what is the issue here?

### Platform

Linux 6.8.0-1009-aws aarch64 GNU/Linux

### Version

uv 0.7.20

### Python version

Python 3.12.7

---

_Label `bug` added by @kayoonkim on 2025-07-19 12:36_

---

_Comment by @notatallshaw on 2025-07-19 14:08_

The issue you've linked is saying there is no version available for Python 3.12, that's one reason you can't install.

If you look at the files available: https://pypi.org/project/onnxsim/#files the pre-compiled packages (wheels ending in ".whl) there is only Python 3.9 to 3.11, and there is no aarch64 available for Linux, so even if you try Python 3.11 it may still not install.

---

_Comment by @notatallshaw on 2025-07-19 14:10_

The thread you linked to also has someone offering their own alternative: https://github.com/tsukumijima/onnx-simplifier-prebuilt. But you will have to trust they know what they're doing (as with any package you install as there is always some security risk involved with installing packages).

---
