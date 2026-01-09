---
number: 9898
title: Error while installing llvmlite on Windows
type: issue
state: closed
author: jgobbo
labels:
  - question
assignees: []
created_at: 2024-12-14T20:24:28Z
updated_at: 2025-01-06T20:55:04Z
url: https://github.com/astral-sh/uv/issues/9898
synced_at: 2026-01-07T13:12:18-06:00
---

# Error while installing llvmlite on Windows

---

_Issue opened by @jgobbo on 2024-12-14 20:24_

I'm trying to add numba as a dependency on my project but it's failing to install llvmlite. I tried the solutions in https://github.com/astral-sh/uv/issues/6281 namely adding "llvmlite >=0.43.0" to constraint-dependencies, but even with the newest version of llvmlite, the installation fails. I've tried clearing the uv cache and updating Visual Studio with no change in the result. Any help is greatly appreciated.

```
> uv add llvmlite
Resolved 51 packages in 231ms
  × Failed to download and build `llvmlite==0.43.0`
  ╰─▶ Build backend failed to build wheel through `build_wheel` (exit code: 1)

      [stdout]
      running bdist_wheel
      C:\Users\gobbo\AppData\Local\uv\cache\builds-v0\.tmpTdaGAp\Scripts\python.exe
      C:\Users\gobbo\AppData\Local\uv\cache\sdists-v6\pypi\llvmlite\0.43.0\FEFslMc92T44SRRD8uayn\src\ffi\build.py
      Trying generator ('Visual Studio 16 2019', 'x64', 'v142')
      Running: cmake -G Visual Studio 16 2019 -A x64 -T v142
      C:\Users\gobbo\AppData\Local\uv\cache\sdists-v6\pypi\llvmlite\0.43.0\FEFslMc92T44SRRD8uayn\src\ffi\dummy

      [stderr]
      Traceback (most recent call last):
        File "C:\Users\gobbo\AppData\Local\uv\cache\sdists-v6\pypi\llvmlite\0.43.0\FEFslMc92T44SRRD8uayn\src\ffi\build.py", line 235, in
      <module>
          main()
          ~~~~^^
        File "C:\Users\gobbo\AppData\Local\uv\cache\sdists-v6\pypi\llvmlite\0.43.0\FEFslMc92T44SRRD8uayn\src\ffi\build.py", line 222, in
      main
          main_windows()
          ~~~~~~~~~~~~^^
        File "C:\Users\gobbo\AppData\Local\uv\cache\sdists-v6\pypi\llvmlite\0.43.0\FEFslMc92T44SRRD8uayn\src\ffi\build.py", line 103, in
      main_windows
          generator = find_windows_generator()
        File "C:\Users\gobbo\AppData\Local\uv\cache\sdists-v6\pypi\llvmlite\0.43.0\FEFslMc92T44SRRD8uayn\src\ffi\build.py", line 91, in
      find_windows_generator
          try_cmake(cmake_dir, build_dir, *generator)
          ~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\gobbo\AppData\Local\uv\cache\sdists-v6\pypi\llvmlite\0.43.0\FEFslMc92T44SRRD8uayn\src\ffi\build.py", line 36, in
      try_cmake
          subprocess.check_call(args)
          ~~~~~~~~~~~~~~~~~~~~~^^^^^^
        File "C:\Users\gobbo\AppData\Roaming\uv\python\cpython-3.13.1-windows-x86_64-none\Lib\subprocess.py", line 414, in check_call
          retcode = call(*popenargs, **kwargs)
        File "C:\Users\gobbo\AppData\Roaming\uv\python\cpython-3.13.1-windows-x86_64-none\Lib\subprocess.py", line 395, in call
          with Popen(*popenargs, **kwargs) as p:
               ~~~~~^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\gobbo\AppData\Roaming\uv\python\cpython-3.13.1-windows-x86_64-none\Lib\subprocess.py", line 1036, in __init__
          self._execute_child(args, executable, preexec_fn, close_fds,
          ~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
                              pass_fds, cwd, env,
                              ^^^^^^^^^^^^^^^^^^^
          ...<5 lines>...
                              gid, gids, uid, umask,
                              ^^^^^^^^^^^^^^^^^^^^^^
                              start_new_session, process_group)
                              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\gobbo\AppData\Roaming\uv\python\cpython-3.13.1-windows-x86_64-none\Lib\subprocess.py", line 1548, in _execute_child
          hp, ht, pid, tid = _winapi.CreateProcess(executable, args,
                             ~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^
                                   # no special security
                                   ^^^^^^^^^^^^^^^^^^^^^
          ...<4 lines>...
                                   cwd,
                                   ^^^^
                                   startupinfo)
                                   ^^^^^^^^^^^^
      FileNotFoundError: [WinError 2] The system cannot find the file specified
      error: command 'C:\\Users\\gobbo\\AppData\\Local\\uv\\cache\\builds-v0\\.tmpTdaGAp\\Scripts\\python.exe' failed with exit code 1

  help: `llvmlite` (v0.43.0) was included because `arpes-uv` (v0.1.0) depends on `llvmlite`
```

---

_Comment by @charliermarsh on 2024-12-15 00:02_

You may want to use an older version of Python, like Python 3.12. llvmlite doesn't ship Python 3.13 wheels yet, so you have to build from source.

---

_Label `question` added by @charliermarsh on 2024-12-15 00:02_

---

_Comment by @lordsoffallen on 2025-01-02 19:39_

I'm coming from Ubuntu, i have rather similar problem. I tried `uv add numba` which for some reason tries to pull `llvmlite==0.36.0` and it didn't work. Then applied `uv add llvmlite` which installs 0.43.0 and then i run `uv add numba` which i get the following line:

```
× Failed to build `numba==0.47.0`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stdout]
      TBB not found
      OpenMP disabled
      running bdist_wheel
      running build
      got version from file /home/ftopal/.cache/uv/sdists-v6/pypi/numba/0.47.0/lkbnSyTJQFW97Jq0rrSPu/src/numba/_version.py
      {'version': '0.47.0', 'full': '4eb9cf875969ef7f099b292e0c5a8bb852f6fc7f'}
      running build_py
      creating build/lib.linux-x86_64-cpython-310/numba
      copying numba/transforms.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/listobject.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/tracing.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/appdirs.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/runtests.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/lowering.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/itanium_mangler.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/interpreter.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/untyped_passes.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/numba_entry.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/serialize.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/__init__.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/dictobject.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/ccallback.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/array_analysis.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/byteflow.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/npdatetime.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/cffi_support.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/callwrapper.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/caching.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/utils.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/compiler_lock.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/funcdesc.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/six.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/charseq.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/dispatcher.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/io_support.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/generators.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/ctypes_support.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/compiler.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/typeinfer.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/macro.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/pylowering.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/dataflow.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_version.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/ir.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/cgutils.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/object_mode_passes.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/pretty_annotate.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/unicode.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/typedobjectutils.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/compiler_machinery.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/findlib.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/typed_passes.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/sigutils.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/numpy_support.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/controlflow.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/__main__.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/entrypoints.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/analysis.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/stencil.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/pythonapi.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/postproc.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/config.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/stencilparfor.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/dummyarray.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/inline_closurecall.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/decorators.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/parfor.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/bytecode.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/errors.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/unittest_support.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_runtests.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/debuginfo.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/consts.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/ir_utils.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/extending.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/numpy_extensions.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/special.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/unicode_support.py -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/withcontexts.py -> build/lib.linux-x86_64-cpython-310/numba
      creating build/lib.linux-x86_64-cpython-310/numba/annotations
      copying numba/annotations/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/annotations
      copying numba/annotations/type_annotations.py -> build/lib.linux-x86_64-cpython-310/numba/annotations
      creating build/lib.linux-x86_64-cpython-310/numba/cext
      copying numba/cext/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/cext
      creating build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/api.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/libdevice.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/intrinsic_wrapper.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/cudaimpl.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/descriptor.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/target.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/cuda_paths.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/args.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/envvars.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/dispatcher.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/compiler.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/nvvmutils.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/initialize.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/testing.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/cudamath.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/codegen.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/vectorizers.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/decorators.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/cudadecl.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/errors.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/stubs.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/simulator_init.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/printimpl.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/device_init.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      copying numba/cuda/random.py -> build/lib.linux-x86_64-cpython-310/numba/cuda
      creating build/lib.linux-x86_64-cpython-310/numba/cuda/cudadrv
      copying numba/cuda/cudadrv/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/cudadrv
      copying numba/cuda/cudadrv/libs.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/cudadrv
      copying numba/cuda/cudadrv/nvvm.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/cudadrv
      copying numba/cuda/cudadrv/error.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/cudadrv
      copying numba/cuda/cudadrv/devices.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/cudadrv
      copying numba/cuda/cudadrv/drvapi.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/cudadrv
      copying numba/cuda/cudadrv/ndarray.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/cudadrv
      copying numba/cuda/cudadrv/autotune.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/cudadrv
      copying numba/cuda/cudadrv/devicearray.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/cudadrv
      copying numba/cuda/cudadrv/enums.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/cudadrv
      copying numba/cuda/cudadrv/driver.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/cudadrv
      creating build/lib.linux-x86_64-cpython-310/numba/cuda/kernels
      copying numba/cuda/kernels/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/kernels
      copying numba/cuda/kernels/transpose.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/kernels
      copying numba/cuda/kernels/reduction.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/kernels
      creating build/lib.linux-x86_64-cpython-310/numba/cuda/simulator
      copying numba/cuda/simulator/api.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/simulator
      copying numba/cuda/simulator/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/simulator
      copying numba/cuda/simulator/kernelapi.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/simulator
      copying numba/cuda/simulator/kernel.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/simulator
      copying numba/cuda/simulator/reduction.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/simulator
      copying numba/cuda/simulator/compiler.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/simulator
      creating build/lib.linux-x86_64-cpython-310/numba/cuda/simulator/cudadrv
      copying numba/cuda/simulator/cudadrv/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/simulator/cudadrv
      copying numba/cuda/simulator/cudadrv/nvvm.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/simulator/cudadrv
      copying numba/cuda/simulator/cudadrv/devices.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/simulator/cudadrv
      copying numba/cuda/simulator/cudadrv/drvapi.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/simulator/cudadrv
      copying numba/cuda/simulator/cudadrv/devicearray.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/simulator/cudadrv
      copying numba/cuda/simulator/cudadrv/driver.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/simulator/cudadrv
      creating build/lib.linux-x86_64-cpython-310/numba/cuda/tests
      copying numba/cuda/tests/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests
      creating build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_cuda_auto_context.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_profiler.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_cuda_devicerecord.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_array_attr.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_select_device.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_reset_device.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_linker.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_host_alloc.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_cuda_driver.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_detect.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_events.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_context_stack.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_cuda_memory.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_ir_patch.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_inline_ptx.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_cuda_libraries.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_pinned.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_cuda_ndarray.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_deallocations.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_cuda_array_slicing.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      copying numba/cuda/tests/cudadrv/test_nvvm_driver.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv
      creating build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv/data
      copying numba/cuda/tests/cudadrv/data/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv/data
      creating build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_autojit.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_serialize.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_device_func.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_minmax.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_lang.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_math.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_globals.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_random.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_blackscholes.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_userexc.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_slicing.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_idiv.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_gufunc_scheduling.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_const_string.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_vectorize.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_constmem.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_localmem.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_nondet.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_powi.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_cuda_array_interface.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_matmul.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_exception.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_reduction.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_multigpu.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_array_methods.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_debuginfo.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_intrinsics.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_array_args.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_multithreads.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_gufunc.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_vectorize_complex.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_montecarlo.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_record_dtype.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_vectorize_scalar_arg.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_sm.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_casting.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_laplace.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_fastmath.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_boolean.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_vectorize_device.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_operator.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_alignment.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_debug.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_inspect.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_forall.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_array.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_py2_div_issue.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_complex_kernel.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_freevar.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_atomics.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_macro.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_vectorize_decor.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_deprecation.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_mandel.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_sync.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_transpose.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_warp_ops.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_errors.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_datetime.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_print.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_complex.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_retrieve_autoconverted_arrays.py ->
      build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_multiprocessing.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_cuda_autojit.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_gufunc_scalar.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      copying numba/cuda/tests/cudapy/test_ipc.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudapy
      creating build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudasim
      copying numba/cuda/tests/cudasim/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudasim
      copying numba/cuda/tests/cudasim/test_cudasim_issues.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudasim
      copying numba/cuda/tests/cudasim/support.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudasim
      creating build/lib.linux-x86_64-cpython-310/numba/cuda/tests/nocuda
      copying numba/cuda/tests/nocuda/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/nocuda
      copying numba/cuda/tests/nocuda/test_library_lookup.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/nocuda
      copying numba/cuda/tests/nocuda/test_nvvm.py -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/nocuda
      creating build/lib.linux-x86_64-cpython-310/numba/datamodel
      copying numba/datamodel/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/datamodel
      copying numba/datamodel/registry.py -> build/lib.linux-x86_64-cpython-310/numba/datamodel
      copying numba/datamodel/testing.py -> build/lib.linux-x86_64-cpython-310/numba/datamodel
      copying numba/datamodel/manager.py -> build/lib.linux-x86_64-cpython-310/numba/datamodel
      copying numba/datamodel/packer.py -> build/lib.linux-x86_64-cpython-310/numba/datamodel
      copying numba/datamodel/models.py -> build/lib.linux-x86_64-cpython-310/numba/datamodel
      creating build/lib.linux-x86_64-cpython-310/numba/help
      copying numba/help/inspector.py -> build/lib.linux-x86_64-cpython-310/numba/help
      copying numba/help/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/help
      creating build/lib.linux-x86_64-cpython-310/numba/jitclass
      copying numba/jitclass/base.py -> build/lib.linux-x86_64-cpython-310/numba/jitclass
      copying numba/jitclass/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/jitclass
      copying numba/jitclass/boxing.py -> build/lib.linux-x86_64-cpython-310/numba/jitclass
      copying numba/jitclass/decorators.py -> build/lib.linux-x86_64-cpython-310/numba/jitclass
      creating build/lib.linux-x86_64-cpython-310/numba/npyufunc
      copying numba/npyufunc/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/npyufunc
      copying numba/npyufunc/ufuncbuilder.py -> build/lib.linux-x86_64-cpython-310/numba/npyufunc
      copying numba/npyufunc/parallel.py -> build/lib.linux-x86_64-cpython-310/numba/npyufunc
      copying numba/npyufunc/array_exprs.py -> build/lib.linux-x86_64-cpython-310/numba/npyufunc
      copying numba/npyufunc/sigparse.py -> build/lib.linux-x86_64-cpython-310/numba/npyufunc
      copying numba/npyufunc/dufunc.py -> build/lib.linux-x86_64-cpython-310/numba/npyufunc
      copying numba/npyufunc/deviceufunc.py -> build/lib.linux-x86_64-cpython-310/numba/npyufunc
      copying numba/npyufunc/wrappers.py -> build/lib.linux-x86_64-cpython-310/numba/npyufunc
      copying numba/npyufunc/decorators.py -> build/lib.linux-x86_64-cpython-310/numba/npyufunc
      copying numba/npyufunc/parfor.py -> build/lib.linux-x86_64-cpython-310/numba/npyufunc
      creating build/lib.linux-x86_64-cpython-310/numba/pycc
      copying numba/pycc/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/pycc
      copying numba/pycc/llvm_types.py -> build/lib.linux-x86_64-cpython-310/numba/pycc
      copying numba/pycc/compiler.py -> build/lib.linux-x86_64-cpython-310/numba/pycc
      copying numba/pycc/cc.py -> build/lib.linux-x86_64-cpython-310/numba/pycc
      copying numba/pycc/platform.py -> build/lib.linux-x86_64-cpython-310/numba/pycc
      copying numba/pycc/decorators.py -> build/lib.linux-x86_64-cpython-310/numba/pycc
      creating build/lib.linux-x86_64-cpython-310/numba/rewrites
      copying numba/rewrites/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/rewrites
      copying numba/rewrites/static_getitem.py -> build/lib.linux-x86_64-cpython-310/numba/rewrites
      copying numba/rewrites/registry.py -> build/lib.linux-x86_64-cpython-310/numba/rewrites
      copying numba/rewrites/static_raise.py -> build/lib.linux-x86_64-cpython-310/numba/rewrites
      copying numba/rewrites/static_binop.py -> build/lib.linux-x86_64-cpython-310/numba/rewrites
      copying numba/rewrites/ir_print.py -> build/lib.linux-x86_64-cpython-310/numba/rewrites
      copying numba/rewrites/macros.py -> build/lib.linux-x86_64-cpython-310/numba/rewrites
      creating build/lib.linux-x86_64-cpython-310/numba/roc
      copying numba/roc/api.py -> build/lib.linux-x86_64-cpython-310/numba/roc
      copying numba/roc/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/roc
      copying numba/roc/descriptor.py -> build/lib.linux-x86_64-cpython-310/numba/roc
      copying numba/roc/mathdecl.py -> build/lib.linux-x86_64-cpython-310/numba/roc
      copying numba/roc/target.py -> build/lib.linux-x86_64-cpython-310/numba/roc
      copying numba/roc/compiler.py -> build/lib.linux-x86_64-cpython-310/numba/roc
      copying numba/roc/mathimpl.py -> build/lib.linux-x86_64-cpython-310/numba/roc
      copying numba/roc/initialize.py -> build/lib.linux-x86_64-cpython-310/numba/roc
      copying numba/roc/enums.py -> build/lib.linux-x86_64-cpython-310/numba/roc
      copying numba/roc/codegen.py -> build/lib.linux-x86_64-cpython-310/numba/roc
      copying numba/roc/dispatch.py -> build/lib.linux-x86_64-cpython-310/numba/roc
      copying numba/roc/vectorizers.py -> build/lib.linux-x86_64-cpython-310/numba/roc
      copying numba/roc/decorators.py -> build/lib.linux-x86_64-cpython-310/numba/roc
      copying numba/roc/hsaimpl.py -> build/lib.linux-x86_64-cpython-310/numba/roc
      copying numba/roc/stubs.py -> build/lib.linux-x86_64-cpython-310/numba/roc
      copying numba/roc/hsadecl.py -> build/lib.linux-x86_64-cpython-310/numba/roc
      copying numba/roc/gcn_occupancy.py -> build/lib.linux-x86_64-cpython-310/numba/roc
      creating build/lib.linux-x86_64-cpython-310/numba/roc/hlc
      copying numba/roc/hlc/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/roc/hlc
      copying numba/roc/hlc/hlc.py -> build/lib.linux-x86_64-cpython-310/numba/roc/hlc
      copying numba/roc/hlc/config.py -> build/lib.linux-x86_64-cpython-310/numba/roc/hlc
      copying numba/roc/hlc/common.py -> build/lib.linux-x86_64-cpython-310/numba/roc/hlc
      copying numba/roc/hlc/libhlc.py -> build/lib.linux-x86_64-cpython-310/numba/roc/hlc
      creating build/lib.linux-x86_64-cpython-310/numba/roc/hsadrv
      copying numba/roc/hsadrv/enums_ext.py -> build/lib.linux-x86_64-cpython-310/numba/roc/hsadrv
      copying numba/roc/hsadrv/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/roc/hsadrv
      copying numba/roc/hsadrv/error.py -> build/lib.linux-x86_64-cpython-310/numba/roc/hsadrv
      copying numba/roc/hsadrv/devices.py -> build/lib.linux-x86_64-cpython-310/numba/roc/hsadrv
      copying numba/roc/hsadrv/drvapi.py -> build/lib.linux-x86_64-cpython-310/numba/roc/hsadrv
      copying numba/roc/hsadrv/devicearray.py -> build/lib.linux-x86_64-cpython-310/numba/roc/hsadrv
      copying numba/roc/hsadrv/enums.py -> build/lib.linux-x86_64-cpython-310/numba/roc/hsadrv
      copying numba/roc/hsadrv/driver.py -> build/lib.linux-x86_64-cpython-310/numba/roc/hsadrv
      creating build/lib.linux-x86_64-cpython-310/numba/roc/tests
      copying numba/roc/tests/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests
      creating build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsadrv
      copying numba/roc/tests/hsadrv/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsadrv
      copying numba/roc/tests/hsadrv/test_driver.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsadrv
      copying numba/roc/tests/hsadrv/test_async.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsadrv
      creating build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/test_autojit.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/test_math.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/test_gufuncbuilding.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/run_far_branch.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/test_matmul.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/test_reduction.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/test_intrinsics.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/test_positioning.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/test_simple.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/test_compiler.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/test_linkage.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/test_scan.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/test_barrier.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/test_memory.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/test_occupancy.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/test_ufuncbuilding.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/test_atomics.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/test_decorator.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/test_async_kernel.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      copying numba/roc/tests/hsapy/test_large_code.py -> build/lib.linux-x86_64-cpython-310/numba/roc/tests/hsapy
      creating build/lib.linux-x86_64-cpython-310/numba/runtime
      copying numba/runtime/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/runtime
      copying numba/runtime/nrt.py -> build/lib.linux-x86_64-cpython-310/numba/runtime
      copying numba/runtime/nrtopt.py -> build/lib.linux-x86_64-cpython-310/numba/runtime
      copying numba/runtime/nrtdynmod.py -> build/lib.linux-x86_64-cpython-310/numba/runtime
      copying numba/runtime/context.py -> build/lib.linux-x86_64-cpython-310/numba/runtime
      creating build/lib.linux-x86_64-cpython-310/numba/scripts
      copying numba/scripts/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/scripts
      copying numba/scripts/generate_lower_listing.py -> build/lib.linux-x86_64-cpython-310/numba/scripts
      creating build/lib.linux-x86_64-cpython-310/numba/servicelib
      copying numba/servicelib/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/servicelib
      copying numba/servicelib/service.py -> build/lib.linux-x86_64-cpython-310/numba/servicelib
      copying numba/servicelib/threadlocal.py -> build/lib.linux-x86_64-cpython-310/numba/servicelib
      creating build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/polynomial.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/callconv.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/cffiimpl.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/base.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/hashing.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/arrayobj.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/npyimpl.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/linalg.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/fastmathpass.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/ufunc_db.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/npdatetime.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/registry.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/boxing.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/quicksort.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/builtins.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/randomimpl.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/cpu_options.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/arraymath.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/descriptors.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/mathimpl.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/optional.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/numbers.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/options.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/literal.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/mergesort.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/imputils.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/iterators.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/dictimpl.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/gdb_hook.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/codegen.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/cpu.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/heapq.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/intrinsics.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/slicing.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/listobj.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/npyfuncs.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/externals.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/setobj.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/cmathimpl.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/removerefctpass.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/enumimpl.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/printimpl.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/tupleobj.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      copying numba/targets/rangeobj.py -> build/lib.linux-x86_64-cpython-310/numba/targets
      creating build/lib.linux-x86_64-cpython-310/numba/testing
      copying numba/testing/loader.py -> build/lib.linux-x86_64-cpython-310/numba/testing
      copying numba/testing/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/testing
      copying numba/testing/notebook.py -> build/lib.linux-x86_64-cpython-310/numba/testing
      copying numba/testing/ddt.py -> build/lib.linux-x86_64-cpython-310/numba/testing
      copying numba/testing/__main__.py -> build/lib.linux-x86_64-cpython-310/numba/testing
      copying numba/testing/main.py -> build/lib.linux-x86_64-cpython-310/numba/testing
      creating build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_builtins.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_serialize.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/dummy_module.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/inlining_usecases.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/jitclass_usecases.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/true_div_usecase.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_slices.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_globals.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_recursion.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_gil.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_random.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_optional.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_blackscholes.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_usecases.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_pycc.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_buffer_protocol.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_runtests.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_array_manipulation.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_np_functions.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_annotations.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_nrt_refct.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_dictimpl.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_sys_stdin_assignment.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_comprehension.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_unpack_sequence.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_del.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_profiler.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_array_constants.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/threading_backend_usecases.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_locals.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/matmul_usecase.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_parfors_caching.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_help.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/ctypes_usecases.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_array_exprs.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_obj_lifetime.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_deprecations.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_multi3.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_intwidth.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_range.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_dicts.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_typeinfer.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_unicode.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_array_attr.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_conversion.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_array_methods.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_debuginfo.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_parallel_backend.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_enums.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_literal_dispatch.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_heapq.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_import.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_polynomial.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/error_usecases.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_typingerror.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_itanium_mangler.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_dictobject.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_datamodel.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_dyn_array.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_parfors.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_copy_propagate.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_vectorization_type_inference.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/recursion_usecases.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_objects.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_nrt.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_linalg.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_dummyarray.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/usecases.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_jitclasses.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_record_dtype.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_listimpl.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/cfunc_cache_usecases.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_codegen.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/annotation_usecases.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_object_mode.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/serialize_usecases.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_casting.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_array_return.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_unicode_names.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_overlap.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_cffi.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_analysis.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_typenames.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_numpyadapt.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_unsafe_intrinsics.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_jitmethod.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_fastmath.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_fancy_indexing.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_target_overloadselector.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_array_analysis.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_typeof.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_withlifting.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_numpy_support.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_jit_module.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_ir_inlining.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_unicode_literals.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_llvm_version_check.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_nested_calls.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_sets.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_extending.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_config.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_storeslice.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_return_values.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_try_except.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_exceptions.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_remove_dead.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_recarray_usecases.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_alignment.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_utils.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_listobject.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_npdatetime.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_interproc.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_debug.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_mangling.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_ir.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_maxmin.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_typedlist.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_chained_assign.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_flow_control.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_func_interface.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_entrypoints.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_practical_lowering_issues.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_sort.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_api.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/pdlike_usecase.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_extending_types.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_array_iterators.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_looplifting.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_cfunc.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_dataflow.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_closure.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_numconv.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_func_lifetime.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_ufuncs.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_ctypes.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_errormodels.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_dyn_func.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_unicode_array.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_python_int.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_array_reductions.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_svml.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/timsort.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_make_function_to_jit_function.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_tracing.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_caching.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_numberctor.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/cache_usecases.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_types.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/complex_usecases.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/orphaned_semaphore_usecase.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_lists.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_typedobjectutils.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_compile_cache.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_gdb.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_mathlib.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_pipeline.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_mixed_tuple_unroller.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_operators.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/overload_usecases.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_iteration.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_auto_constants.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_generators.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_hashing.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_typeconv.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_inlining.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_print.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_extended_arg.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_boundscheck.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/support.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_indexing.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_wrapper.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_stencils.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_numbers.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/enum_usecases.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_errorhandling.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/compile_with_pycc.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_complex.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_threadsafety.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_nan.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/cffi_usecases.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_warnings.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_dispatcher.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_support.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_mandelbrot.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/parfors_cache_usecases.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_cgutils.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_cli.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_tuples.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_ir_utils.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_map_filter_reduce.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      copying numba/tests/test_compiler_lock.py -> build/lib.linux-x86_64-cpython-310/numba/tests
      creating build/lib.linux-x86_64-cpython-310/numba/tests/npyufunc
      copying numba/tests/npyufunc/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/tests/npyufunc
      copying numba/tests/npyufunc/test_parallel_ufunc_issues.py -> build/lib.linux-x86_64-cpython-310/numba/tests/npyufunc
      copying numba/tests/npyufunc/test_gufunc.py -> build/lib.linux-x86_64-cpython-310/numba/tests/npyufunc
      copying numba/tests/npyufunc/test_parallel_low_work.py -> build/lib.linux-x86_64-cpython-310/numba/tests/npyufunc
      copying numba/tests/npyufunc/test_ufuncbuilding.py -> build/lib.linux-x86_64-cpython-310/numba/tests/npyufunc
      copying numba/tests/npyufunc/test_parallel_env_variable.py -> build/lib.linux-x86_64-cpython-310/numba/tests/npyufunc
      copying numba/tests/npyufunc/test_ufunc.py -> build/lib.linux-x86_64-cpython-310/numba/tests/npyufunc
      copying numba/tests/npyufunc/test_dufunc.py -> build/lib.linux-x86_64-cpython-310/numba/tests/npyufunc
      copying numba/tests/npyufunc/test_caching.py -> build/lib.linux-x86_64-cpython-310/numba/tests/npyufunc
      copying numba/tests/npyufunc/test_vectorize_decor.py -> build/lib.linux-x86_64-cpython-310/numba/tests/npyufunc
      copying numba/tests/npyufunc/cache_usecases.py -> build/lib.linux-x86_64-cpython-310/numba/tests/npyufunc
      copying numba/tests/npyufunc/test_errors.py -> build/lib.linux-x86_64-cpython-310/numba/tests/npyufunc
      creating build/lib.linux-x86_64-cpython-310/numba/typeconv
      copying numba/typeconv/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/typeconv
      copying numba/typeconv/castgraph.py -> build/lib.linux-x86_64-cpython-310/numba/typeconv
      copying numba/typeconv/rules.py -> build/lib.linux-x86_64-cpython-310/numba/typeconv
      copying numba/typeconv/typeconv.py -> build/lib.linux-x86_64-cpython-310/numba/typeconv
      creating build/lib.linux-x86_64-cpython-310/numba/typed
      copying numba/typed/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/typed
      copying numba/typed/typedlist.py -> build/lib.linux-x86_64-cpython-310/numba/typed
      copying numba/typed/typeddict.py -> build/lib.linux-x86_64-cpython-310/numba/typed
      creating build/lib.linux-x86_64-cpython-310/numba/types
      copying numba/types/misc.py -> build/lib.linux-x86_64-cpython-310/numba/types
      copying numba/types/functions.py -> build/lib.linux-x86_64-cpython-310/numba/types
      copying numba/types/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/types
      copying numba/types/abstract.py -> build/lib.linux-x86_64-cpython-310/numba/types
      copying numba/types/scalars.py -> build/lib.linux-x86_64-cpython-310/numba/types
      copying numba/types/iterators.py -> build/lib.linux-x86_64-cpython-310/numba/types
      copying numba/types/common.py -> build/lib.linux-x86_64-cpython-310/numba/types
      copying numba/types/npytypes.py -> build/lib.linux-x86_64-cpython-310/numba/types
      copying numba/types/containers.py -> build/lib.linux-x86_64-cpython-310/numba/types
      creating build/lib.linux-x86_64-cpython-310/numba/typing
      copying numba/typing/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/typing
      copying numba/typing/randomdecl.py -> build/lib.linux-x86_64-cpython-310/numba/typing
      copying numba/typing/arraydecl.py -> build/lib.linux-x86_64-cpython-310/numba/typing
      copying numba/typing/listdecl.py -> build/lib.linux-x86_64-cpython-310/numba/typing
      copying numba/typing/mathdecl.py -> build/lib.linux-x86_64-cpython-310/numba/typing
      copying numba/typing/npdatetime.py -> build/lib.linux-x86_64-cpython-310/numba/typing
      copying numba/typing/ctypes_utils.py -> build/lib.linux-x86_64-cpython-310/numba/typing
      copying numba/typing/setdecl.py -> build/lib.linux-x86_64-cpython-310/numba/typing
      copying numba/typing/builtins.py -> build/lib.linux-x86_64-cpython-310/numba/typing
      copying numba/typing/npydecl.py -> build/lib.linux-x86_64-cpython-310/numba/typing
      copying numba/typing/collections.py -> build/lib.linux-x86_64-cpython-310/numba/typing
      copying numba/typing/cffi_utils.py -> build/lib.linux-x86_64-cpython-310/numba/typing
      copying numba/typing/bufproto.py -> build/lib.linux-x86_64-cpython-310/numba/typing
      copying numba/typing/enumdecl.py -> build/lib.linux-x86_64-cpython-310/numba/typing
      copying numba/typing/cmathdecl.py -> build/lib.linux-x86_64-cpython-310/numba/typing
      copying numba/typing/templates.py -> build/lib.linux-x86_64-cpython-310/numba/typing
      copying numba/typing/context.py -> build/lib.linux-x86_64-cpython-310/numba/typing
      copying numba/typing/typeof.py -> build/lib.linux-x86_64-cpython-310/numba/typing
      copying numba/typing/dictdecl.py -> build/lib.linux-x86_64-cpython-310/numba/typing
      creating build/lib.linux-x86_64-cpython-310/numba/unsafe
      copying numba/unsafe/bytes.py -> build/lib.linux-x86_64-cpython-310/numba/unsafe
      copying numba/unsafe/__init__.py -> build/lib.linux-x86_64-cpython-310/numba/unsafe
      copying numba/unsafe/nrt.py -> build/lib.linux-x86_64-cpython-310/numba/unsafe
      copying numba/unsafe/eh.py -> build/lib.linux-x86_64-cpython-310/numba/unsafe
      copying numba/unsafe/refcount.py -> build/lib.linux-x86_64-cpython-310/numba/unsafe
      copying numba/unsafe/ndarray.py -> build/lib.linux-x86_64-cpython-310/numba/unsafe
      copying numba/unsafe/numbers.py -> build/lib.linux-x86_64-cpython-310/numba/unsafe
      copying numba/unsafe/tuple.py -> build/lib.linux-x86_64-cpython-310/numba/unsafe
      copying numba/_npymath_exports.c -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_typeof.c -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_dispatcher.c -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_dynfuncmod.c -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_dynfunc.c -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_math_c99.c -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_helpermod.c -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_random.c -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_lapack.c -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_hashtable.c -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/mviewbuf.c -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_helperlib.c -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/capsulethunk.h -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_pymodule.h -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/mathnames.h -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_unicodetype_db.h -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_hashtable.h -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_arraystruct.h -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_typeof.h -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_dispatcher.h -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_numba_common.h -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/_math_c99.h -> build/lib.linux-x86_64-cpython-310/numba
      copying numba/annotations/template.html -> build/lib.linux-x86_64-cpython-310/numba/annotations
      copying numba/cext/listobject.c -> build/lib.linux-x86_64-cpython-310/numba/cext
      copying numba/cext/utils.c -> build/lib.linux-x86_64-cpython-310/numba/cext
      copying numba/cext/dictobject.c -> build/lib.linux-x86_64-cpython-310/numba/cext
      copying numba/cext/dictobject.h -> build/lib.linux-x86_64-cpython-310/numba/cext
      copying numba/cext/cext.h -> build/lib.linux-x86_64-cpython-310/numba/cext
      copying numba/cext/listobject.h -> build/lib.linux-x86_64-cpython-310/numba/cext
      copying numba/cuda/tests/cudadrv/data/jitlink.ptx -> build/lib.linux-x86_64-cpython-310/numba/cuda/tests/cudadrv/data
      copying numba/pycc/modulemixin.c -> build/lib.linux-x86_64-cpython-310/numba/pycc
      copying numba/runtime/_nrt_python.c -> build/lib.linux-x86_64-cpython-310/numba/runtime
      copying numba/runtime/nrt.c -> build/lib.linux-x86_64-cpython-310/numba/runtime
      copying numba/runtime/_nrt_pythonmod.c -> build/lib.linux-x86_64-cpython-310/numba/runtime
      copying numba/runtime/nrt.h -> build/lib.linux-x86_64-cpython-310/numba/runtime
      copying numba/runtime/nrt_external.h -> build/lib.linux-x86_64-cpython-310/numba/runtime
      copying numba/targets/cmdlang.gdb -> build/lib.linux-x86_64-cpython-310/numba/targets
      creating build/lib.linux-x86_64-cpython-310/numba/tests/pycc_distutils_usecase
      copying numba/tests/pycc_distutils_usecase/setup_distutils.py ->
      build/lib.linux-x86_64-cpython-310/numba/tests/pycc_distutils_usecase
      copying numba/tests/pycc_distutils_usecase/source_module.py ->
      build/lib.linux-x86_64-cpython-310/numba/tests/pycc_distutils_usecase
      copying numba/tests/pycc_distutils_usecase/setup_setuptools.py ->
      build/lib.linux-x86_64-cpython-310/numba/tests/pycc_distutils_usecase
      running build_ext
      building 'numba._dynfunc' extension
      INFO: C compiler: cc -pthread -Wno-unused-result -Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -fPIC
      -fPIC

      creating build/temp.linux-x86_64-cpython-310/numba
      INFO: compile options: '-I/home/ftopal/.cache/uv/builds-v0/.tmpKE8rKb/include
      -I/home/ftopal/.local/share/uv/python/cpython-3.10.16-linux-x86_64-gnu/include/python3.10 -c'
      extra options: '-g'
      INFO: cc: numba/_dynfuncmod.c
      INFO: cc -pthread -shared -Wl,--exclude-libs,ALL build/temp.linux-x86_64-cpython-310/numba/_dynfuncmod.o -o
      build/lib.linux-x86_64-cpython-310/numba/_dynfunc.cpython-310-x86_64-linux-gnu.so
      building 'numba._dispatcher' extension
      INFO: C compiler: cc -pthread -Wno-unused-result -Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -fPIC
      -fPIC

      creating build/temp.linux-x86_64-cpython-310/numba/typeconv
      INFO: compile options: '-I/home/ftopal/.cache/uv/builds-v0/.tmpKE8rKb/lib/python3.10/site-packages/numpy/_core/include
      -I/home/ftopal/.cache/uv/builds-v0/.tmpKE8rKb/include
      -I/home/ftopal/.local/share/uv/python/cpython-3.10.16-linux-x86_64-gnu/include/python3.10 -c'
      INFO: cc: numba/_dispatcher.c
      INFO: cc: numba/_dispatcherimpl.cpp
      INFO: cc: numba/_typeof.c
      INFO: cc: numba/typeconv/typeconv.cpp
      INFO: cc: numba/_hashtable.c

      [stderr]
      <string>:96: DeprecationWarning:

        `numpy.distutils` is deprecated since NumPy 1.23.0, as a result
        of the deprecation of `distutils` itself. It will be removed for
        Python >= 3.12. For older Python versions it will remain present.
        It is recommended to use `setuptools < 60.0` for those Python versions.
        For more details, see:
          https://numpy.org/devdocs/reference/distutils_status_migration.html


      numba/_dispatcher.c: In function ‘call_trace’:
      numba/_dispatcher.c:26:13: error: ‘PyThreadState’ {aka ‘struct _ts’} has no member named ‘use_tracing’; did you mean
      ‘tracing’?
         26 |     tstate->use_tracing = 0;
            |             ^~~~~~~~~~~
            |             tracing
      numba/_dispatcher.c:28:13: error: ‘PyThreadState’ {aka ‘struct _ts’} has no member named ‘use_tracing’; did you mean
      ‘tracing’?
         28 |     tstate->use_tracing = ((tstate->c_tracefunc != NULL)
            |             ^~~~~~~~~~~
            |             tracing
      numba/_dispatcher.c: In function ‘call_cfunc’:
      numba/_dispatcher.c:301:17: error: ‘PyThreadState’ {aka ‘struct _ts’} has no member named ‘use_tracing’; did you mean
      ‘tracing’?
        301 |     if (tstate->use_tracing && tstate->c_profilefunc)
            |                 ^~~~~~~~~~~
            |                 tracing
      numba/_dispatcher.c: In function ‘Dispatcher_call’:
      numba/_dispatcher.c:507:13: error: ‘PyThreadState’ {aka ‘struct _ts’} has no member named ‘use_tracing’; did you mean
      ‘tracing’?
        507 |     if (ts->use_tracing && ts->c_profilefunc)
            |             ^~~~~~~~~~~
            |             tracing
      numba/_dispatcher.c: In function ‘call_cfunc’:
      numba/_dispatcher.c:349:1: warning: control reaches end of non-void function [-Wreturn-type]
        349 | }
            | ^
      numba/_typeof.c: In function ‘compute_dtype_fingerprint’:
      numba/_typeof.c:231:55: error: ‘PyArray_Descr’ {aka ‘struct _PyArray_Descr’} has no member named ‘c_metadata’
        231 |         md = &(((PyArray_DatetimeDTypeMetaData *)descr->c_metadata)->meta);
            |                                                       ^~
      error: Command "cc -pthread -Wno-unused-result -Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall
      -fPIC -fPIC -I/home/ftopal/.cache/uv/builds-v0/.tmpKE8rKb/lib/python3.10/site-packages/numpy/_core/include
      -I/home/ftopal/.cache/uv/builds-v0/.tmpKE8rKb/include
      -I/home/ftopal/.local/share/uv/python/cpython-3.10.16-linux-x86_64-gnu/include/python3.10 -c numba/_dispatcher.c -o
      build/temp.linux-x86_64-cpython-310/numba/_dispatcher.o" failed with exit status 1

      hint: This usually indicates a problem with the package or the build environment.
  help: `numba` (v0.47.0) was included because `projx` (v0.1) depends on `numba`
```

How can we get this installed? I am at a loss here, don't know where the issue is coming from

---

_Comment by @lordsoffallen on 2025-01-02 19:50_

Update, this was installing old version of numba because i had `numpy>=2.2.1` installed. So i enabled `numpy>=2.0.0` in my project and after that it work fine by installing 0.60.0 version of numba 🎉 

---

_Comment by @zanieb on 2025-01-02 20:01_

Thanks for following up!

I wonder if there's a place for this in https://docs.astral.sh/uv/reference/build_failures/

---

_Comment by @lordsoffallen on 2025-01-02 20:06_

> Thanks for following up!
> 
> I wonder if there's a place for this in https://docs.astral.sh/uv/reference/build_failures/

I actually think putting a section on how to install these common libs would be cool. Like there is pytorch section, I believe we can put them somewhere for better visibility. I now suspect for `uv add numba` not working in the first place was that `numpy >= 2.2.1` dependency in my project toml file. I am sure many people overlook that and create issues here. Like FAQ of errors we face and common found solutions  

---

_Closed by @zanieb on 2025-01-06 20:55_

---

_Referenced in [astral-sh/uv#11636](../../astral-sh/uv/issues/11636.md) on 2025-02-19 20:01_

---
