```yaml
number: 16507
title: "python 3.13 not satisfy request `>=3.11`"
type: issue
state: closed
author: thqq479
labels:
  - question
assignees: []
created_at: 2025-10-30T03:09:50Z
updated_at: 2025-11-03T08:53:54Z
url: https://github.com/astral-sh/uv/issues/16507
synced_at: 2026-01-12T16:02:33Z
```

# python 3.13 not satisfy request `>=3.11`

---

_@thqq479_

### Summary

DEBUG uv 0.7.2
DEBUG Found `cpython-3.13.9+freethreaded-linux-x86_64-gnu` at `/usr/local/bin/python3.13t` (search path)
DEBUG Skipping interpreter at `/usr/local/bin/python3.13t` from search path: does not satisfy request `>=3.11`
DEBUG Found `cpython-3.13.9+freethreaded-linux-x86_64-gnu` at `/usr/local/bin/python3.13` (search path)
DEBUG Skipping interpreter at `/usr/local/bin/python3.13` from search path: does not satisfy request `>=3.11`

### Platform

ubuntu 22.04

### Version

0.7.2

### Python version

3.13

---

_Label `bug` added by @thqq479 on 2025-10-30 03:09_

---

_Comment by @thqq479 on 2025-10-30 03:11_

Why doesn't the 3.13-nogil version I installed work?

---

_Comment by @samypr100 on 2025-10-30 05:04_

Does the issue occur in a newer uv version (e.g. 0.9.6)?

---

_Comment by @thqq479 on 2025-10-30 06:50_

> Does the issue occur in a newer uv version (e.g. 0.9.6)?

Still the same in 0.9.6

---

_Comment by @thqq479 on 2025-10-30 13:45_

> Does the issue occur in a newer uv version (e.g. 0.9.6)?

I found that only the no-gil version is causing issues. How can I resolve this? Can I compile with 3.13 but run with 3.13-nogil?

---

_Comment by @charliermarsh on 2025-10-30 13:47_

I believe this is the same as #16434.

---

_Comment by @zanieb on 2025-10-30 13:48_

We require opt-in to use a free-threaded 3.13 interpreter, e.g., `-p 3.13t`. We don't require this for 3.14+, so I don't think we'll change our handling of 3.13.

---

_Label `bug` removed by @zanieb on 2025-10-30 13:49_

---

_Label `question` added by @zanieb on 2025-10-30 13:49_

---

_Comment by @thqq479 on 2025-10-30 13:55_

> We require opt-in to use a free-threaded 3.13 interpreter, e.g., `-p 3.13t`. We don't require this for 3.14+, so I don't think we'll change our handling of 3.13.

Thanks Could you please tell me where exactly I can add the -p 3.13t parameter? Could you provide an example so I can try it out? @zanieb 


---

_Comment by @zanieb on 2025-10-30 14:05_

Well, you didn't share what commands you're running so I can't really tell you where to add it. Add it to the command that's skipping your desired Python environment though? Or just set `UV_PYTHON=3.13t`



---

_Comment by @thqq479 on 2025-10-30 14:17_

> Well, you didn't share what commands you're running so I can't really tell you where to add it. Add it to the command that's skipping your desired Python environment though? Or just set `UV_PYTHON=3.13t`

I was trying to compile sgl-kernel using 3.13t, and actually made a uv call in this line:
https://github.com/sgl-project/sglang/blob/b0d25e72c401f37b55d689ddbf05b8c583afe854/sgl-kernel/Makefile#L28

and i got this log:

DEBUG uv 0.7.2
DEBUG Found cpython-3.13.9+freethreaded-linux-x86_64-gnu at /usr/local/bin/python3.13t (search path)
DEBUG Skipping interpreter at /usr/local/bin/python3.13t from search path: does not satisfy request >=3.11
DEBUG Found cpython-3.13.9+freethreaded-linux-x86_64-gnu at /usr/local/bin/python3.13 (search path)
DEBUG Skipping interpreter at /usr/local/bin/python3.13 from search path: does not satisfy request >=3.11

---

_Comment by @samypr100 on 2025-11-01 06:12_

@thqq479 For reference, I didn't encounter issues building [sgl-kernel](https://github.com/sgl-project/sglang/tree/v0.5.4.post2) on a clean environment (e.g. docker).

```
root@f78735faf093:~/sglang/sgl-kernel# make build
DEBUG uv 0.9.7
DEBUG Acquired shared lock for `/root/.cache/uv`
DEBUG Found workspace root: `/root/sglang/sgl-kernel`
DEBUG Adding root workspace member: `/root/sglang/sgl-kernel`
DEBUG Searching for Python 3.13t in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.13.9+freethreaded-linux-x86_64-gnu` at `/root/sglang/sgl-kernel/.venv/bin/python3` (virtual environment)
DEBUG Using request timeout of 30s
DEBUG Not using uv build backend direct build of `.`, pyproject.toml does not match: The value for `build_system.build-backend` should be `"uv_build"`, not `"scikit_build_core.build"`
Building wheel...
DEBUG No workspace root found, using project root
DEBUG Proceeding without build isolation
DEBUG Calling `scikit_build_core.build.build_wheel("/root/sglang/sgl-kernel/dist", {"build-dir":"build"}, None)`
*** scikit-build-core 0.11.6 using CMake 3.31.6 (wheel)
*** Configuring CMake...
loading initial cache file build/CMakeInit.txt
-- /root/sglang/sgl-kernel/.venv/lib/python3.13t/site-packages/torch/share/cmake
CMake Warning at /root/sglang/sgl-kernel/.venv/lib/python3.13t/site-packages/torch/share/cmake/Torch/TorchConfig.cmake:22 (message):
  static library kineto_LIBRARY-NOTFOUND not found.
Call Stack (most recent call first):
  /root/sglang/sgl-kernel/.venv/lib/python3.13t/site-packages/torch/share/cmake/Torch/TorchConfig.cmake:125 (append_torchlib_if_found)
  CMakeLists.txt:19 (find_package)


-- Enabling macro: SGLANG_CPU_FP8_CVT_FTZ
CMake Warning (dev) at /usr/share/cmake-3.31/Modules/FindPython/Support.cmake:4174 (cmake_parse_arguments):
  The USE_SABI keyword was followed by an empty string or no value at all.
  Policy CMP0174 is not set, so cmake_parse_arguments() will unset the
  PYTHON_ADD_LIBRARY_USE_SABI variable rather than setting it to an empty
  string.
Call Stack (most recent call first):
  /usr/share/cmake-3.31/Modules/FindPython.cmake:692 (__Python_add_library)
  CMakeLists.txt:77 (Python_add_library)
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Configuring done (2.3s)
-- Generating done (0.0s)
-- Build files have been written to: /root/sglang/sgl-kernel/build
*** Building project with Unix Makefiles...
gmake[1]: Entering directory '/root/sglang/sgl-kernel/build'
[  5%] Building CXX object CMakeFiles/common_ops.dir/decode.cpp.o
[ 10%] Building CXX object CMakeFiles/common_ops.dir/gemm_int8.cpp.o
[ 15%] Building CXX object CMakeFiles/common_ops.dir/shm.cpp.o
[ 21%] Building CXX object CMakeFiles/common_ops.dir/torch_extension_cpu.cpp.o
[ 26%] Linking CXX shared module common_ops.cpython-313t-x86_64-linux-gnu.so
[100%] Built target common_ops
gmake[1]: Leaving directory '/root/sglang/sgl-kernel/build'
*** Installing project into wheel...
-- Install configuration: "Release"
-- Installing: /tmp/tmph9xrm7hd/wheel/platlib/sgl_kernel/common_ops.cpython-313t-x86_64-linux-gnu.so
-- Set non-toolchain portion of runtime path of "/tmp/tmph9xrm7hd/wheel/platlib/sgl_kernel/common_ops.cpython-313t-x86_64-linux-gnu.so" to ""
*** Making wheel...
*** Created sgl_kernel-0.3.16.post4-cp313-cp313t-linux_x86_64.whl
Successfully built dist/sgl_kernel-0.3.16.post4-cp313-cp313t-linux_x86_64.whl
```

---

_Comment by @thqq479 on 2025-11-03 08:53_

> [@thqq479](https://github.com/thqq479) For reference, I didn't encounter issues building [sgl-kernel](https://github.com/sgl-project/sglang/tree/v0.5.4.post2) on a clean environment (e.g. docker).
> 
> ```
> root@f78735faf093:~/sglang/sgl-kernel# make build
> DEBUG uv 0.9.7
> DEBUG Acquired shared lock for `/root/.cache/uv`
> DEBUG Found workspace root: `/root/sglang/sgl-kernel`
> DEBUG Adding root workspace member: `/root/sglang/sgl-kernel`
> DEBUG Searching for Python 3.13t in virtual environments, managed installations, or search path
> DEBUG Found `cpython-3.13.9+freethreaded-linux-x86_64-gnu` at `/root/sglang/sgl-kernel/.venv/bin/python3` (virtual environment)
> DEBUG Using request timeout of 30s
> DEBUG Not using uv build backend direct build of `.`, pyproject.toml does not match: The value for `build_system.build-backend` should be `"uv_build"`, not `"scikit_build_core.build"`
> Building wheel...
> DEBUG No workspace root found, using project root
> DEBUG Proceeding without build isolation
> DEBUG Calling `scikit_build_core.build.build_wheel("/root/sglang/sgl-kernel/dist", {"build-dir":"build"}, None)`
> *** scikit-build-core 0.11.6 using CMake 3.31.6 (wheel)
> *** Configuring CMake...
> loading initial cache file build/CMakeInit.txt
> -- /root/sglang/sgl-kernel/.venv/lib/python3.13t/site-packages/torch/share/cmake
> CMake Warning at /root/sglang/sgl-kernel/.venv/lib/python3.13t/site-packages/torch/share/cmake/Torch/TorchConfig.cmake:22 (message):
>   static library kineto_LIBRARY-NOTFOUND not found.
> Call Stack (most recent call first):
>   /root/sglang/sgl-kernel/.venv/lib/python3.13t/site-packages/torch/share/cmake/Torch/TorchConfig.cmake:125 (append_torchlib_if_found)
>   CMakeLists.txt:19 (find_package)
> 
> 
> -- Enabling macro: SGLANG_CPU_FP8_CVT_FTZ
> CMake Warning (dev) at /usr/share/cmake-3.31/Modules/FindPython/Support.cmake:4174 (cmake_parse_arguments):
>   The USE_SABI keyword was followed by an empty string or no value at all.
>   Policy CMP0174 is not set, so cmake_parse_arguments() will unset the
>   PYTHON_ADD_LIBRARY_USE_SABI variable rather than setting it to an empty
>   string.
> Call Stack (most recent call first):
>   /usr/share/cmake-3.31/Modules/FindPython.cmake:692 (__Python_add_library)
>   CMakeLists.txt:77 (Python_add_library)
> This warning is for project developers.  Use -Wno-dev to suppress it.
> 
> -- Configuring done (2.3s)
> -- Generating done (0.0s)
> -- Build files have been written to: /root/sglang/sgl-kernel/build
> *** Building project with Unix Makefiles...
> gmake[1]: Entering directory '/root/sglang/sgl-kernel/build'
> [  5%] Building CXX object CMakeFiles/common_ops.dir/decode.cpp.o
> [ 10%] Building CXX object CMakeFiles/common_ops.dir/gemm_int8.cpp.o
> [ 15%] Building CXX object CMakeFiles/common_ops.dir/shm.cpp.o
> [ 21%] Building CXX object CMakeFiles/common_ops.dir/torch_extension_cpu.cpp.o
> [ 26%] Linking CXX shared module common_ops.cpython-313t-x86_64-linux-gnu.so
> [100%] Built target common_ops
> gmake[1]: Leaving directory '/root/sglang/sgl-kernel/build'
> *** Installing project into wheel...
> -- Install configuration: "Release"
> -- Installing: /tmp/tmph9xrm7hd/wheel/platlib/sgl_kernel/common_ops.cpython-313t-x86_64-linux-gnu.so
> -- Set non-toolchain portion of runtime path of "/tmp/tmph9xrm7hd/wheel/platlib/sgl_kernel/common_ops.cpython-313t-x86_64-linux-gnu.so" to ""
> *** Making wheel...
> *** Created sgl_kernel-0.3.16.post4-cp313-cp313t-linux_x86_64.whl
> Successfully built dist/sgl_kernel-0.3.16.post4-cp313-cp313t-linux_x86_64.whl
> ```

Thank you. After adding the parameter -p 3.13t, it worked for me

---

_Closed by @thqq479 on 2025-11-03 08:53_

---
