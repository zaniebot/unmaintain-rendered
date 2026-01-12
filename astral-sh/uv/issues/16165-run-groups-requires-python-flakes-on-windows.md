```yaml
number: 16165
title: "`run_groups_requires_python` flakes on Windows"
type: issue
state: open
author: zanieb
labels:
  - ci-flake
assignees: []
created_at: 2025-10-07T21:44:20Z
updated_at: 2025-11-28T09:41:09Z
url: https://github.com/astral-sh/uv/issues/16165
synced_at: 2026-01-12T16:02:25Z
```

# `run_groups_requires_python` flakes on Windows

---

_@zanieb_

``` 
    thread 'run::run_groups_requires_python' panicked at crates\uv\tests\it\run.rs:5113:5:
    DEBUG uv 0.9.0 (30771d47f 2025-10-07)
    TRACE Checking shared lock for `V:\uv-tmp\uv\tests\.tmpadVooJ\cache` at `V:\uv-tmp\uv\tests\.tmpadVooJ\cache\.lock`
    DEBUG Acquired shared lock for `V:\uv-tmp\uv\tests\.tmpadVooJ\cache`
    DEBUG Found project root: `V:\uv-tmp\uv\tests\.tmpadVooJ\temp`
    DEBUG No workspace root found, using project root
    DEBUG Discovered project `project` at: V:\uv-tmp\uv\tests\.tmpadVooJ\temp
    TRACE Checking lock for `V:\uv-tmp\uv\tests\.tmpadVooJ\temp` at `V:\uv-tmp\uv-3f33e78c625082d2.lock`
    DEBUG Acquired lock for `V:\uv-tmp\uv\tests\.tmpadVooJ\temp`
    DEBUG No Python version file found in workspace: V:\uv-tmp\uv\tests\.tmpadVooJ\temp
    DEBUG Using Python request `>=3.12` from `requires-python` metadata
    DEBUG Checking for Python environment at: `.venv`
    TRACE Found cached interpreter info for Python 3.12.9, skipping query of: .venv\Scripts\python.exe
    DEBUG The interpreter in the project environment has a different version (3.12.9) than it was created with (3.13.2)
    DEBUG Searching for Python >=3.12 in managed installations, search path, or registry
    DEBUG Searching for managed installations at `V:\uv-tmp\uv\tests\.tmpadVooJ\home\uv\python`
    TRACE Searching PATH for executables: python3.exe, python.exe
    TRACE Checking `PATH` directory for interpreters: C:\Users\Administrator\AppData\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none
    TRACE Found possible Python executable: C:\Users\Administrator\AppData\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none\python.exe
    TRACE Found cached interpreter info for Python 3.11.11, skipping query of: C:\Users\Administrator\AppData\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none\python.exe
    DEBUG Found `cpython-3.11.11-windows-x86_64-none` at `C:\Users\Administrator\AppData\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none\python.exe` (first executable in the search path)
    DEBUG Skipping interpreter at `C:\Users\Administrator\AppData\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none\python.exe` from first executable in the search path: does not satisfy request `>=3.12`
    TRACE Checking `PATH` directory for interpreters: C:\Users\Administrator\AppData\Roaming\uv\python\cpython-3.12.9-windows-x86_64-none
    TRACE Found possible Python executable: C:\Users\Administrator\AppData\Roaming\uv\python\cpython-3.12.9-windows-x86_64-none\python.exe
    TRACE Found cached interpreter info for Python 3.12.9, skipping query of: C:\Users\Administrator\AppData\Roaming\uv\python\cpython-3.12.9-windows-x86_64-none\python.exe
    DEBUG Found `cpython-3.12.9-windows-x86_64-none` at `C:\Users\Administrator\AppData\Roaming\uv\python\cpython-3.12.9-windows-x86_64-none\python.exe` (search path)
    Using CPython 3.12.9 interpreter at: C:\Users\Administrator\AppData\Roaming\uv\python\cpython-3.12.9-windows-x86_64-none\python.exe
    Removed virtual environment at: .venv
    Creating virtual environment at: .venv
    DEBUG Using base executable for virtual environment: C:\Users\Administrator\AppData\Roaming\uv\python\cpython-3.12.9-windows-x86_64-none\python.exe
    DEBUG Released lock at `V:\uv-tmp\uv-3f33e78c625082d2.lock`
    TRACE Checking lock for `.venv` at `.venv\.lock`
    DEBUG Acquired lock for `.venv`
    DEBUG Using request timeout of 30s
    DEBUG Found static `pyproject.toml` for: project @ file:///V:/uv-tmp/uv/tests/.tmpadVooJ/temp
    DEBUG No workspace root found, using project root
    DEBUG Existing `uv.lock` satisfies workspace requirements
    Resolved 6 packages in 2ms
    DEBUG Using request timeout of 30s
    DEBUG Registry requirement already cached: sniffio==1.3.1
    DEBUG Registry requirement already cached: typing-extensions==4.10.0
    TRACE Extracting file name=PackageName("sniffio")
    TRACE Extracting file name=PackageName("typing-extensions")
    TRACE Extracted 5 files name=PackageName("typing-extensions")
    TRACE No entrypoints name=PackageName("typing-extensions")
    TRACE No data name=PackageName("typing-extensions")
    TRACE Writing installer metadata name=PackageName("typing-extensions")
    TRACE Writing record name=PackageName("typing-extensions")
    TRACE Extracted 13 files name=PackageName("sniffio")
    TRACE No entrypoints name=PackageName("sniffio")
    TRACE No data name=PackageName("sniffio")
    TRACE Writing installer metadata name=PackageName("sniffio")
    TRACE Writing record name=PackageName("sniffio")
    Installed 2 packages in 6ms
     + sniffio==1.3.1
     + typing-extensions==4.10.0
    DEBUG Released lock at `V:\uv-tmp\uv\tests\.tmpadVooJ\temp\.venv\.lock`
    DEBUG Using Python 3.12.9 interpreter at: V:\uv-tmp\uv\tests\.tmpadVooJ\temp\.venv\Scripts\python.exe
    DEBUG Running `python -c import typing_extensions`
    DEBUG Command exited with code: 0
    DEBUG Released lock at `V:\uv-tmp\uv\tests\.tmpadVooJ\cache\.lock`

    stack backtrace:
       0: std::panicking::begin_panic_handler
                 at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\std\src\panicking.rs:697
       1: core::panicking::panic_fmt
                 at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\core\src\panicking.rs:75
       2: core::panicking::panic_display<enum2$<alloc::borrow::Cow<str$> > >
                 at /rustc/1159e78c4747b02ef996e55082b704c09b970588\library\core\src\panicking.rs:268
       3: it::run::run_groups_requires_python::panic_cold_display<enum2$<alloc::borrow::Cow<str$> > >
                 at /rustc/1159e78c4747b02ef996e55082b704c09b970588\library\core\src\panic.rs:100
       4: it::run::run_groups_requires_python
                 at .\tests\it\run.rs:5113
       5: it::run::run_groups_requires_python::closure$0
                 at .\tests\it\run.rs:5019
       6: core::ops::function::FnOnce::call_once<it::run::run_groups_requires_python::closure_env$0,tuple$<> >
                 at /rustc/1159e78c4747b02ef996e55082b704c09b970588\library\core\src\ops\function.rs:253
       7: core::ops::function::FnOnce::call_once
                 at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\core\src\ops\function.rs:253
    note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

https://github.com/astral-sh/uv/actions/runs/18326035686/job/52192751847?pr=16161

---

_Label `ci-flake` added by @zanieb on 2025-10-07 21:44_

---

_Comment by @zanieb on 2025-10-07 22:13_

https://github.com/astral-sh/uv/blob/37b3557dabe20d9a37567691c454cd03aa36f3be/crates/uv/tests/it/run.rs#L5103-L5117

---

_Comment by @zanieb on 2025-10-07 22:16_

Concerningly this trace looks the same as the one purportedly fixed by https://github.com/astral-sh/uv/pull/14331 and previously reported in https://github.com/astral-sh/uv/issues/14160 

---

_Comment by @konstin on 2025-11-28 09:41_

Another one: https://github.com/astral-sh/uv/actions/runs/19759562669/job/56618121438?pr=16780

---
