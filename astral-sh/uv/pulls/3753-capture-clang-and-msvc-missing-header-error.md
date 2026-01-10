```yaml
number: 3753
title: Capture clang and msvc missing header error
type: pull_request
state: merged
author: SigureMo
labels:
  - error messages
assignees: []
merged: true
base: main
head: capture-clang-and-msvc-missing-header-error
created_at: 2024-05-22T18:31:48Z
updated_at: 2024-05-22T18:41:21Z
url: https://github.com/astral-sh/uv/pull/3753
synced_at: 2026-01-10T14:32:20Z
```

# Capture clang and msvc missing header error

---

_Pull request opened by @SigureMo on 2024-05-22 18:31_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #3732

The full log for MSVC and clang is as follows:

Clang (on macOS):

```
pip install pygraphviz
Collecting pygraphviz
  Downloading pygraphviz-1.13.tar.gz (104 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 104.6/104.6 kB 964.1 kB/s eta 0:00:00
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Installing backend dependencies ... done
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: pygraphviz
  Building wheel for pygraphviz (pyproject.toml) ... error
  error: subprocess-exited-with-error

  × Building wheel for pygraphviz (pyproject.toml) did not run successfully.
  │ exit code: 1
  ╰─> [63 lines of output]
      running bdist_wheel
      running build
      running build_py
      creating build
      creating build/lib.macosx-14.0-arm64-cpython-311
      creating build/lib.macosx-14.0-arm64-cpython-311/pygraphviz
      copying pygraphviz/scraper.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz
      copying pygraphviz/graphviz.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz
      copying pygraphviz/__init__.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz
      copying pygraphviz/agraph.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz
      copying pygraphviz/testing.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz
      creating build/lib.macosx-14.0-arm64-cpython-311/pygraphviz/tests
      copying pygraphviz/tests/test_unicode.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz/tests
      copying pygraphviz/tests/test_scraper.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz/tests
      copying pygraphviz/tests/test_readwrite.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz/tests
      copying pygraphviz/tests/test_string.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz/tests
      copying pygraphviz/tests/__init__.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz/tests
      copying pygraphviz/tests/test_html.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz/tests
      copying pygraphviz/tests/test_node_attributes.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz/tests
      copying pygraphviz/tests/test_drawing.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz/tests
      copying pygraphviz/tests/test_repr_mimebundle.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz/tests
      copying pygraphviz/tests/test_subgraph.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz/tests
      copying pygraphviz/tests/test_close.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz/tests
      copying pygraphviz/tests/test_edge_attributes.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz/tests
      copying pygraphviz/tests/test_clear.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz/tests
      copying pygraphviz/tests/test_layout.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz/tests
      copying pygraphviz/tests/test_attribute_defaults.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz/tests
      copying pygraphviz/tests/test_graph.py -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz/tests
      running egg_info
      writing pygraphviz.egg-info/PKG-INFO
      writing dependency_links to pygraphviz.egg-info/dependency_links.txt
      writing top-level names to pygraphviz.egg-info/top_level.txt
      reading manifest file 'pygraphviz.egg-info/SOURCES.txt'
      reading manifest template 'MANIFEST.in'
      warning: no files found matching '*.swg'
      warning: no files found matching '*.png' under directory 'doc'
      warning: no files found matching '*.html' under directory 'doc'
      warning: no files found matching '*.txt' under directory 'doc'
      warning: no files found matching '*.css' under directory 'doc'
      warning: no previously-included files matching '*~' found anywhere in distribution
      warning: no previously-included files matching '*.pyc' found anywhere in distribution
      warning: no previously-included files matching '.svn' found anywhere in distribution
      no previously-included directories found matching 'doc/build'
      adding license file 'LICENSE'
      writing manifest file 'pygraphviz.egg-info/SOURCES.txt'
      copying pygraphviz/graphviz.i -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz
      copying pygraphviz/graphviz_wrap.c -> build/lib.macosx-14.0-arm64-cpython-311/pygraphviz
      running build_ext
      building 'pygraphviz._graphviz' extension
      creating build/temp.macosx-14.0-arm64-cpython-311
      creating build/temp.macosx-14.0-arm64-cpython-311/pygraphviz
      clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk -I/opt/homebrew/opt/openssl@3.0/include -DSWIG_PYTHON_STRICT_BYTE_CHAR -I/Users/nyakku/Projects/yutto/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c pygraphviz/graphviz_wrap.c -o build/temp.macosx-14.0-arm64-cpython-311/pygraphviz/graphviz_wrap.o
      pygraphviz/graphviz_wrap.c:9:9: warning: 'SWIG_PYTHON_STRICT_BYTE_CHAR' macro redefined [-Wmacro-redefined]
          9 | #define SWIG_PYTHON_STRICT_BYTE_CHAR
            |         ^
      <command line>:2:9: note: previous definition is here
          2 | #define SWIG_PYTHON_STRICT_BYTE_CHAR 1
            |         ^
      pygraphviz/graphviz_wrap.c:3023:10: fatal error: 'graphviz/cgraph.h' file not found
       3023 | #include "graphviz/cgraph.h"
            |          ^~~~~~~~~~~~~~~~~~~
      1 warning and 1 error generated.
      error: command '/opt/homebrew/opt/llvm/bin/clang' failed with exit code 1
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
  ERROR: Failed building wheel for pygraphviz
Failed to build pygraphviz
ERROR: Could not build wheels for pygraphviz, which is required to install pyproject.toml-based projects

[notice] A new release of pip is available: 23.3.1 -> 24.0
[notice] To update, run: pip install --upgrade pip
```

MSVC (on Windows):

```
pip install pygraphviz
Collecting pygraphviz
  Downloading pygraphviz-1.13.tar.gz (104 kB)
     |████████████████████████████████| 104 kB 595 kB/s
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Installing backend dependencies ... done
    Preparing wheel metadata ... done
Building wheels for collected packages: pygraphviz
  Building wheel for pygraphviz (PEP 517) ... error
  ERROR: Command errored out with exit status 1:
   command: 'C:\Python310\python.exe' 'C:\Python310\lib\site-packages\pip\_vendor\pep517\in_process\_in_process.py' build_wheel 'C:\Users\PADDLE~2\AppData\Local\Temp\tmpbl6h1qvd'
       cwd: C:\Users\paddle_dev2\AppData\Local\Temp\pip-install-2e4v3xju\pygraphviz_034fe74e775e43e3935b6a88e2cd4761
  Complete output (58 lines):
  running bdist_wheel
  running build
  running build_py
  creating build
  creating build\lib.win-amd64-cpython-310
  creating build\lib.win-amd64-cpython-310\pygraphviz
  copying pygraphviz\agraph.py -> build\lib.win-amd64-cpython-310\pygraphviz
  copying pygraphviz\graphviz.py -> build\lib.win-amd64-cpython-310\pygraphviz
  copying pygraphviz\scraper.py -> build\lib.win-amd64-cpython-310\pygraphviz
  copying pygraphviz\testing.py -> build\lib.win-amd64-cpython-310\pygraphviz
  copying pygraphviz\__init__.py -> build\lib.win-amd64-cpython-310\pygraphviz
  creating build\lib.win-amd64-cpython-310\pygraphviz\tests
  copying pygraphviz\tests\test_attribute_defaults.py -> build\lib.win-amd64-cpython-310\pygraphviz\tests
  copying pygraphviz\tests\test_clear.py -> build\lib.win-amd64-cpython-310\pygraphviz\tests
  copying pygraphviz\tests\test_close.py -> build\lib.win-amd64-cpython-310\pygraphviz\tests
  copying pygraphviz\tests\test_drawing.py -> build\lib.win-amd64-cpython-310\pygraphviz\tests
  copying pygraphviz\tests\test_edge_attributes.py -> build\lib.win-amd64-cpython-310\pygraphviz\tests
  copying pygraphviz\tests\test_graph.py -> build\lib.win-amd64-cpython-310\pygraphviz\tests
  copying pygraphviz\tests\test_html.py -> build\lib.win-amd64-cpython-310\pygraphviz\tests
  copying pygraphviz\tests\test_layout.py -> build\lib.win-amd64-cpython-310\pygraphviz\tests
  copying pygraphviz\tests\test_node_attributes.py -> build\lib.win-amd64-cpython-310\pygraphviz\tests
  copying pygraphviz\tests\test_readwrite.py -> build\lib.win-amd64-cpython-310\pygraphviz\tests
  copying pygraphviz\tests\test_repr_mimebundle.py -> build\lib.win-amd64-cpython-310\pygraphviz\tests
  copying pygraphviz\tests\test_scraper.py -> build\lib.win-amd64-cpython-310\pygraphviz\tests
  copying pygraphviz\tests\test_string.py -> build\lib.win-amd64-cpython-310\pygraphviz\tests
  copying pygraphviz\tests\test_subgraph.py -> build\lib.win-amd64-cpython-310\pygraphviz\tests
  copying pygraphviz\tests\test_unicode.py -> build\lib.win-amd64-cpython-310\pygraphviz\tests
  copying pygraphviz\tests\__init__.py -> build\lib.win-amd64-cpython-310\pygraphviz\tests
  running egg_info
  writing pygraphviz.egg-info\PKG-INFO
  writing dependency_links to pygraphviz.egg-info\dependency_links.txt
  writing top-level names to pygraphviz.egg-info\top_level.txt
  reading manifest file 'pygraphviz.egg-info\SOURCES.txt'
  reading manifest template 'MANIFEST.in'
  warning: no files found matching '*.swg'
  warning: no files found matching '*.png' under directory 'doc'
  warning: no files found matching '*.html' under directory 'doc'
  warning: no files found matching '*.txt' under directory 'doc'
  warning: no files found matching '*.css' under directory 'doc'
  warning: no previously-included files matching '*~' found anywhere in distribution
  warning: no previously-included files matching '*.pyc' found anywhere in distribution
  warning: no previously-included files matching '.svn' found anywhere in distribution
  no previously-included directories found matching 'doc\build'
  adding license file 'LICENSE'
  writing manifest file 'pygraphviz.egg-info\SOURCES.txt'
  copying pygraphviz\graphviz.i -> build\lib.win-amd64-cpython-310\pygraphviz
  copying pygraphviz\graphviz_wrap.c -> build\lib.win-amd64-cpython-310\pygraphviz
  running build_ext
  building 'pygraphviz._graphviz' extension
  creating build\temp.win-amd64-cpython-310
  creating build\temp.win-amd64-cpython-310\Release
  creating build\temp.win-amd64-cpython-310\Release\pygraphviz
  "C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\bin\HostX86\x64\cl.exe" /c /nologo /O2 /W3 /GL /DNDEBUG /MD -DSWIG_PYTHON_STRICT_BYTE_CHAR -DGVDLL -IC:\Python310\include -IC:\Python310\Include "-IC:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\ATLMFC\include" "-IC:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\include" "-IC:\Program Files (x86)\Windows Kits\10\include\10.0.19041.0\ucrt" "-IC:\Program Files (x86)\Windows Kits\10\include\10.0.19041.0\shared" "-IC:\Program Files (x86)\Windows Kits\10\include\10.0.19041.0\um" "-IC:\Program Files (x86)\Windows Kits\10\include\10.0.19041.0\winrt" "-IC:\Program Files (x86)\Windows Kits\10\include\10.0.19041.0\cppwinrt" "-IC:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\ATLMFC\include" "-IC:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\include" "-IC:\Program Files (x86)\Windows Kits\NETFXSDK\4.6.1\include\um" "-IC:\Program Files (x86)\Windows Kits\10\include\10.0.19041.0\ucrt" "-IC:\Program Files (x86)\Windows Kits\10\include\10.0.19041.0\shared" "-IC:\Program Files (x86)\Windows Kits\10\include\10.0.19041.0\um" "-IC:\Program Files (x86)\Windows Kits\10\include\10.0.19041.0\winrt" "-IC:\Program Files (x86)\Windows Kits\10\include\10.0.19041.0\cppwinrt" /Tcpygraphviz/graphviz_wrap.c /Fobuild\temp.win-amd64-cpython-310\Release\pygraphviz/graphviz_wrap.obj
  graphviz_wrap.c
  pygraphviz/graphviz_wrap.c(9): warning C4005: 'SWIG_PYTHON_STRICT_BYTE_CHAR': macro redefinition
  pygraphviz/graphviz_wrap.c: note: see previous definition of 'SWIG_PYTHON_STRICT_BYTE_CHAR'
  pygraphviz/graphviz_wrap.c(3023): fatal error C1083: Cannot open include file: 'graphviz/cgraph.h': No such file or directory
  error: command 'C:\\Program Files (x86)\\Microsoft Visual Studio\\2017\\Community\\VC\\Tools\\MSVC\\14.16.27023\\bin\\HostX86\\x64\\cl.exe' failed with exit code 2
  ----------------------------------------
  ERROR: Failed building wheel for pygraphviz
Failed to build pygraphviz
ERROR: Could not build wheels for pygraphviz which use PEP 517 and cannot be installed directly
WARNING: You are using pip version 21.2.3; however, version 24.0 is available.
You should consider upgrading via the 'C:\Python310\python.exe -m pip install --upgrade pip' command.
```

## Test Plan

Running `cargo run -- pip install pygraphviz` and expecting to get `Caused by: This error likely indicates that you need to install a library that provides "graphviz/cgraph.h" for pygraphviz==1.13`

---

_Review requested from @konstin by @zanieb on 2024-05-22 18:36_

---

_Assigned to @konstin by @zanieb on 2024-05-22 18:36_

---

_@charliermarsh approved on 2024-05-22 18:40_

Nice, thanks!

---

_Merged by @charliermarsh on 2024-05-22 18:40_

---

_Closed by @charliermarsh on 2024-05-22 18:40_

---

_Label `error messages` added by @charliermarsh on 2024-05-22 18:40_

---

_Branch deleted on 2024-05-22 18:41_

---
