```yaml
number: 11924
title: "Can't build `scikit-build-core` project with Fortran component"
type: issue
state: open
author: ZedThree
labels:
  - bug
assignees: []
created_at: 2025-03-03T14:13:10Z
updated_at: 2025-03-03T14:13:10Z
url: https://github.com/astral-sh/uv/issues/11924
synced_at: 2026-01-12T16:00:49Z
```

# Can't build `scikit-build-core` project with Fortran component

---

_@ZedThree_

### Summary

Steps to reproduce:

1. Create a new project using `scikit-build-core`: `uv init --build-backend scikit-build-core`
2. Add `Fortran` to the list of languages in the `project` call: `sed -i 's/CXX/CXX Fortran/'`
3. Build: `uv pip install .`

This fails with:

```
Resolved 1 package in 1ms
  × Failed to build `testproject @ file:///tmp/testproject`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `scikit_build_core.build.build_wheel` failed (exit status: 1)

      [stdout]
      *** scikit-build-core 0.11.0 using CMake 3.31.5 (wheel)
      *** Configuring CMake...
      loading initial cache file build/cp313-cp313-linux_x86_64/CMakeInit.txt
      -- The CXX compiler identification is Clang 19.1.7
      -- The Fortran compiler identification is unknown
      -- Detecting CXX compiler ABI info
      -- Detecting CXX compiler ABI info - done
      -- Check for working CXX compiler: /usr/bin/clang++ - skipped
      -- Detecting CXX compile features
      -- Detecting CXX compile features - done
      -- Detecting Fortran compiler ABI info
      -- Configuring incomplete, errors occurred!

      [stderr]
      CMake Error: Error required internal CMake variable not set, cmake may not be built correctly.
      Missing variable is:
      CMAKE_Fortran_PREPROCESS_SOURCE
      CMake Error at /usr/share/cmake/Modules/CMakeDetermineCompilerABI.cmake:74 (try_compile):
        Failed to generate test project build system.
      Call Stack (most recent call first):
        /usr/share/cmake/Modules/CMakeTestFortranCompiler.cmake:20 (CMAKE_DETERMINE_COMPILER_ABI)
        CMakeLists.txt:2 (project)
```

The relevant bit is: `The Fortran compiler identification is unknown`. I notice that the C++ compiler is picked up as Clang, so I guess this is either because:

a) `uv` is setting the `CXX` environment variable (or `CMAKE_C_COMPILER`?) to `clang++`, and not setting `FC`
b) `uv` is setting `FC` to `flang` but I don't have that installed

Importantly, the project builds correctly without `uv`, and picks up the GNU compilers.

How do I override whatever `uv` is doing here? I can set `--config-settings=cmake.args=-DCMAKE_Fortran_COMPILER=gfortran`, but is there a nicer way for end users?

### Platform

Linux 6.13.3-1-default x86_64 GNU/Linux

### Version

uv 0.5.23

### Python version

_No response_

---

_Label `bug` added by @ZedThree on 2025-03-03 14:13_

---
