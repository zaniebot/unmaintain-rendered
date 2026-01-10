```yaml
number: 14092
title: WIP extra-build-dependencies
type: pull_request
state: closed
author: Gankra
labels:
  - enhancement
assignees: []
draft: true
base: main
head: gankra/add-build-deps
created_at: 2025-06-16T21:31:56Z
updated_at: 2025-07-18T17:37:19Z
url: https://github.com/astral-sh/uv/pull/14092
synced_at: 2026-01-10T06:53:01Z
```

# WIP extra-build-dependencies

---

_Pull request opened by @Gankra on 2025-06-16 21:31_

This introduces a global setting 

```
[tool.uv.extra-build-dependencies]
pytest = ["setuptools"]
```

Which forces extra requirements into a build-isolated sdist build for any package that has the given name. Included is a test the demonstrates the feature is at all useful/functional using an example from 

* #2252 

`no-build-isolation-package` was used as the template for all the places this setting should be threaded, as they both mess with the isolated build of a package by-name. There's a very high chance this implementation is insufficient, as I didn't go out of my way to do anything with caching (but maybe I ended up shoving it somewhere that does).

---

_Converted to draft by @Gankra on 2025-06-16 21:32_

---

_@Gankra reviewed on 2025-06-16 21:33_

---

_Review comment by @Gankra on `crates/uv-build-frontend/src/lib.rs`:535 on 2025-06-16 21:33_

Happy path where I'm confident Sources/Indexes are appropriately handled.

---

_@Gankra reviewed on 2025-06-16 21:34_

---

_Review comment by @Gankra on `crates/uv-build-frontend/src/lib.rs`:626 on 2025-06-16 21:34_

Sad path 1 where I probably need to handle sources where previously they were always irrelevant (pyproject.toml but missing `[build-system]`)

---

_@Gankra reviewed on 2025-06-16 21:35_

---

_Review comment by @Gankra on `crates/uv-build-frontend/src/lib.rs`:651 on 2025-06-16 21:35_

Sad path 2 where I am also probably mishandling sources/indexes (no pyproject.toml).

---

_@Gankra reviewed on 2025-06-16 21:36_

---

_Review comment by @Gankra on `crates/uv/tests/it/sync.rs`:1605 on 2025-06-16 21:36_

Glorious success!

---

_Comment by @notatallshaw on 2025-06-17 01:03_

I think this effectively fixes https://github.com/astral-sh/uv/issues/12447

---

_Comment by @konstin on 2025-06-17 14:38_

The test fails to pass for me locally, with the same error as when I try to install asttext==0.9.2 on its own. Does it need a specific, older compiler version?

```
$ uv pip install fasttext==0.9.2 --no-build-isolation-package fasttext
Resolved 4 packages in 3ms
  × Failed to build `fasttext==0.9.2`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stdout]
      running bdist_wheel
      running build
      running build_py
      copying python/fasttext_module/fasttext/FastText.py -> build/lib.linux-x86_64-cpython-313/fasttext
      copying python/fasttext_module/fasttext/__init__.py -> build/lib.linux-x86_64-cpython-313/fasttext
      copying python/fasttext_module/fasttext/util/util.py -> build/lib.linux-x86_64-cpython-313/fasttext/util
      copying python/fasttext_module/fasttext/util/__init__.py -> build/lib.linux-x86_64-cpython-313/fasttext/util
      copying python/fasttext_module/fasttext/tests/test_script.py -> build/lib.linux-x86_64-cpython-313/fasttext/tests
      copying python/fasttext_module/fasttext/tests/test_configurations.py -> build/lib.linux-x86_64-cpython-313/fasttext/tests
      copying python/fasttext_module/fasttext/tests/__init__.py -> build/lib.linux-x86_64-cpython-313/fasttext/tests
      running build_ext
      c++ -pthread -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fPIC -fPIC -I/home/konsti/projects/uv/.venv/include
      -I/home/konsti/.local/share/uv/python/cpython-3.13.2-linux-x86_64-gnu/include/python3.13 -c /tmp/tmpl6kbi6ua.cpp -o tmp/tmpl6kbi6ua.o -std=c++14
      c++ -pthread -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fPIC -fPIC -I/home/konsti/projects/uv/.venv/include
      -I/home/konsti/.local/share/uv/python/cpython-3.13.2-linux-x86_64-gnu/include/python3.13 -c /tmp/tmp91txd3ad.cpp -o tmp/tmp91txd3ad.o
      -fvisibility=hidden
      building 'fasttext_pybind' extension
      c++ -pthread -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall
      -fPIC -fPIC -I/home/konsti/projects/uv/.venv/lib/python3.13/site-packages/pybind11/include
      -I/home/konsti/projects/uv/.venv/lib/python3.13/site-packages/pybind11/include -Isrc -I/home/konsti/projects/uv/.venv/include
      -I/home/konsti/.local/share/uv/python/cpython-3.13.2-linux-x86_64-gnu/include/python3.13 -c python/fasttext_module/fasttext/pybind/fasttext_pybind.cc
      -o build/temp.linux-x86_64-cpython-313/python/fasttext_module/fasttext/pybind/fasttext_pybind.o -DVERSION_INFO=\"0.9.2\" -std=c++14
      -fvisibility=hidden
      c++ -pthread -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall
      -fPIC -fPIC -I/home/konsti/projects/uv/.venv/lib/python3.13/site-packages/pybind11/include
      -I/home/konsti/projects/uv/.venv/lib/python3.13/site-packages/pybind11/include -Isrc -I/home/konsti/projects/uv/.venv/include
      -I/home/konsti/.local/share/uv/python/cpython-3.13.2-linux-x86_64-gnu/include/python3.13 -c src/args.cc -o
      build/temp.linux-x86_64-cpython-313/src/args.o -DVERSION_INFO=\"0.9.2\" -std=c++14 -fvisibility=hidden

      [stderr]
      /home/konsti/projects/uv/.venv/lib/python3.13/site-packages/setuptools/dist.py:599: SetuptoolsDeprecationWarning: Invalid dash-separated key
      'description-file' in 'metadata' (setup.cfg), please use the underscore name 'description_file' instead.
      !!

              ********************************************************************************
              Usage of dash-separated 'description-file' will not be supported in future
              versions. Please use the underscore name 'description_file' instead.
              (Affected: fasttext).

              By 2026-Mar-03, you need to update your project and remove deprecated calls
              or your builds will no longer be supported.

              See https://setuptools.pypa.io/en/latest/userguide/declarative_config.html for details.
              ********************************************************************************

      !!
        opt = self._enforce_underscore(opt, section)
      /home/konsti/projects/uv/.venv/lib/python3.13/site-packages/setuptools/dist.py:759: SetuptoolsDeprecationWarning: License classifiers are
      deprecated.
      !!

              ********************************************************************************
              Please consider removing the following classifiers in favor of a SPDX license expression:

              License :: OSI Approved :: MIT License

              See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license for details.
              ********************************************************************************

      !!
        self._finalize_license_expression()
      python/fasttext_module/fasttext/pybind/fasttext_pybind.cc: In lambda function:
      python/fasttext_module/fasttext/pybind/fasttext_pybind.cc:345:35: warning: comparison of integer expressions of different signedness: ‘int32_t’
      {aka ‘int’} and ‘std::vector<long int>::size_type’ {aka ‘long unsigned int’} [-Wsign-compare]
        345 |             for (int32_t i = 0; i < vocab_freq.size(); i++) {
            |                                 ~~^~~~~~~~~~~~~~~~~~~
      python/fasttext_module/fasttext/pybind/fasttext_pybind.cc: In lambda function:
      python/fasttext_module/fasttext/pybind/fasttext_pybind.cc:359:35: warning: comparison of integer expressions of different signedness: ‘int32_t’
      {aka ‘int’} and ‘std::vector<long int>::size_type’ {aka ‘long unsigned int’} [-Wsign-compare]
        359 |             for (int32_t i = 0; i < labels_freq.size(); i++) {
            |                                 ~~^~~~~~~~~~~~~~~~~~~~
      src/args.cc: In member function ‘void fasttext::Args::parseArgs(const std::vector<std::__cxx11::basic_string<char> >&)’:
      src/args.cc:120:23: warning: comparison of integer expressions of different signedness: ‘int’ and ‘std::vector<std::__cxx11::basic_string<char>
      >::size_type’ {aka ‘long unsigned int’} [-Wsign-compare]
        120 |   for (int ai = 2; ai < args.size(); ai += 2) {
            |                    ~~~^~~~~~~~~~~~~
      src/args.cc:221:19: warning: catching polymorphic type ‘class std::out_of_range’ by value [-Wcatch-value=]
        221 |     } catch (std::out_of_range) {
            |                   ^~~~~~~~~~~~
      src/args.cc: In member function ‘int64_t fasttext::Args::getAutotuneModelSize() const’:
      src/args.cc:468:3: error: ‘uint64_t’ was not declared in this scope
        468 |   uint64_t multiplier = 1;
            |   ^~~~~~~~
      src/args.cc:17:1: note: ‘uint64_t’ is defined in header ‘<cstdint>’; did you forget to ‘#include <cstdint>’?
         16 | #include <unordered_map>
        +++ |+#include <cstdint>
         17 |
      src/args.cc:471:5: error: ‘multiplier’ was not declared in this scope
        471 |     multiplier = units[lastCharacter];
            |     ^~~~~~~~~~
      src/args.cc:474:11: error: expected ‘;’ before ‘size’
        474 |   uint64_t size = 0;
            |           ^~~~~
            |           ;
      src/args.cc:478:5: error: ‘size’ was not declared in this scope
        478 |     size = std::stol(modelSize, &nonNumericCharacter);
            |     ^~~~
      src/args.cc:490:10: error: ‘size’ was not declared in this scope
        490 |   return size * multiplier;
            |          ^~~~
      src/args.cc:490:17: error: ‘multiplier’ was not declared in this scope
        490 |   return size * multiplier;
            |                 ^~~~~~~~~~
      error: command '/usr/bin/c++' failed with exit code 1

      hint: This usually indicates a problem with the package or the build environment.
```

```
$ gcc --version
gcc (Ubuntu 13.3.0-6ubuntu2~24.04) 13.3.0
Copyright (C) 2023 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
$ clang --version
Ubuntu clang version 18.1.3 (1ubuntu1)
Target: x86_64-pc-linux-gnu
Thread model: posix
InstalledDir: /usr/bin
```

---

_Comment by @Gankra on 2025-06-17 16:16_

Oh hrm that's annoying, there's other packages I could use for this problem though.

---

_Comment by @konstin on 2025-06-17 16:25_

We should pick something where the successful build is still fast, we had problems with slow builds in test the past. The fastest should be handrolling something with `build-system.backend-path` where our custom build system imports a marker package and then forwards us to hatchling the regular way.

---

_Comment by @Florent-H on 2025-07-03 18:45_

Any updates on this pull request? I am having build isolation issues (#2252). Thanks for your time and effort!

---

_Label `enhancement` added by @konstin on 2025-07-16 07:11_

---

_Assigned to @zanieb by @zanieb on 2025-07-18 13:18_

---

_Closed by @zanieb on 2025-07-18 17:37_

---
