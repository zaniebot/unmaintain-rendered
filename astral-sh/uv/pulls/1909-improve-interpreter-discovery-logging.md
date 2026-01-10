```yaml
number: 1909
title: Improve interpreter discovery logging
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - windows
assignees: []
merged: true
base: main
head: konsti/better-interpreter-detection-logging
created_at: 2024-02-23T12:17:35Z
updated_at: 2024-02-23T18:43:47Z
url: https://github.com/astral-sh/uv/pull/1909
synced_at: 2026-01-10T14:54:43Z
```

# Improve interpreter discovery logging

---

_Pull request opened by @konstin on 2024-02-23 12:17_

We had several cases where interpreter discovery fails. This PR improves the verbose output to ensure interpreter discovery is debuggable for a user.

In the process, i removed the custom gourgeist logic for the uv_interpreter logic.

**venv creation**

```
$ uv venv -v -p 3.10
 uv_interpreter::python_query::find_requested_python request=3.10
      0.002389s   0ms DEBUG uv_interpreter::python_query Starting interpreter discovery for Python 3.10
   uv_interpreter::python_query::windows::py_list_paths
      0.016288s  14ms DEBUG uv_interpreter::interpreter Probing interpreter info for: C:\Users\Ferris\AppData\Local\Programs\Python\Python312\python.exe
      0.072860s  70ms DEBUG uv_interpreter::interpreter Found Python 3.12.1 for: C:\Users\Ferris\AppData\Local\Programs\Python\Python312\python.exe
      0.074303s  72ms DEBUG uv_interpreter::interpreter Probing interpreter info for: C:\Users\Ferris\AppData\Local\Programs\Python\Python38\python.exe
      0.134311s 132ms DEBUG uv_interpreter::interpreter Found Python 3.8.10 for: C:\Users\Ferris\AppData\Local\Programs\Python\Python38\python.exe
  x No Python 3.10 found through `py --list-paths` or in `PATH`. Is Python 3.10 installed?
error: process didn't exit successfully: `target\debug\uv.exe venv -v -p 3.10` (exit code: 1)
```

```
$ uv venv -v -p 3.10
 uv_interpreter::python_query::find_requested_python request=3.10
      0.001889s   0ms DEBUG uv_interpreter::python_query Starting interpreter discovery for Python 3.10
   uv_interpreter::python_query::windows::py_list_paths
      0.021488s  19ms DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.12.1, skipping probing: C:\Users\Ferris\AppData\Local\Programs\Python\Python312\python.exe
      0.021945s  20ms DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.8.10, skipping probing: C:\Users\Ferris\AppData\Local\Programs\Python\Python38\python.exe
  x No Python 3.10 found through `py --list-paths` or in `PATH`. Is Python 3.10 installed?
error: process didn't exit successfully: `target\debug\uv.exe venv -v -p 3.10` (exit code: 1)
```

```
$ uv venv -v -p 3.8
 uv_interpreter::python_query::find_requested_python request=3.8
      0.001896s   0ms DEBUG uv_interpreter::python_query Starting interpreter discovery for Python 3.8
   uv_interpreter::python_query::windows::py_list_paths
      0.013541s  11ms DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.8.10, skipping probing: C:\Users\Ferris\AppData\Local\Programs\Python\Python38\python.exe
Using Python 3.8.10 interpreter at C:\Users\Ferris\AppData\Local\Programs\Python\Python38\python.exe
Creating virtualenv at: .venv
Activate with: .venv\Scripts\activate
```

```
$ uv venv -v -p 3.12
 uv_interpreter::python_query::find_requested_python request=3.12
      0.001741s   0ms DEBUG uv_interpreter::python_query Starting interpreter discovery for Python 3.12
   uv_interpreter::python_query::windows::py_list_paths
      0.012807s  11ms DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.12.1, skipping probing: C:\Users\Ferris\AppData\Local\Programs\Python\Python312\python.exe
Using Python 3.12.1 interpreter at C:\Users\Ferris\AppData\Local\Programs\Python\Python312\python.exe
Creating virtualenv at: .venv
Activate with: .venv\Scripts\activate
```

**pip compile**

```
$ uv pip compile -v .\scripts\requirements\black.in
   uv::requirements::from_source source=.\scripts\requirements\black.in
   uv_interpreter::interpreter::find_best python_version=None
        0.002071s   0ms DEBUG uv_interpreter::interpreter Starting interpreter discovery for active Python
        0.002220s   0ms DEBUG uv_interpreter::virtual_env Found a virtualenv named .venv at: C:\Users\Ferris\projects\uv\.venv
        0.002483s   0ms DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.12.1, skipping probing: C:\Users\Ferris\projects\uv\.venv\Scripts\python.exe
      0.002581s DEBUG uv::commands::pip_compile Using Python 3.12.1 interpreter at C:\Users\Ferris\projects\uv\.venv\Scripts\python.exe for builds
```

```
$ uv pip compile -p 3.8 -v .\scripts\requirements\black.in
   uv::requirements::from_source source=.\scripts\requirements\black.in
   uv_interpreter::interpreter::find_best python_version=Some(PythonVersion(StringVersion { string: "3.8", version: "3.8" }))
        0.002001s   0ms DEBUG uv_interpreter::interpreter Starting interpreter discovery for Python 3.8
        0.002146s   0ms DEBUG uv_interpreter::virtual_env Found a virtualenv named .venv at: C:\Users\Ferris\projects\uv\.venv
        0.002378s   0ms DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.12.1, skipping probing: C:\Users\Ferris\projects\uv\.venv\Scripts\python.exe
     uv_interpreter::python_query::find_requested_python request=3.8
          0.002509s   0ms DEBUG uv_interpreter::python_query Starting interpreter discovery for Python 3.8
       uv_interpreter::python_query::windows::py_list_paths
          0.015989s  13ms DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.8.10, skipping probing: C:\Users\Ferris\AppData\Local\Programs\Python\Python38\python.exe
      0.016144s DEBUG uv::commands::pip_compile Using Python 3.8.10 interpreter at C:\Users\Ferris\AppData\Local\Programs\Python\Python38\python.exe for builds

```

```
$ uv pip compile -p 3.10 -v .\scripts\requirements\black.in
   uv::requirements::from_source source=.\scripts\requirements\black.in
   uv_interpreter::interpreter::find_best python_version=Some(PythonVersion(StringVersion { string: "3.10", version: "3.10" }))
        0.002086s   0ms DEBUG uv_interpreter::interpreter Starting interpreter discovery for Python 3.10
        0.002234s   0ms DEBUG uv_interpreter::virtual_env Found a virtualenv named .venv at: C:\Users\Ferris\projects\uv\.venv
        0.002462s   0ms DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.12.1, skipping probing: C:\Users\Ferris\projects\uv\.venv\Scripts\python.exe
     uv_interpreter::python_query::find_requested_python request=3.10
          0.002589s   0ms DEBUG uv_interpreter::python_query Starting interpreter discovery for Python 3.10
       uv_interpreter::python_query::windows::py_list_paths
          0.017299s  14ms DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.12.1, skipping probing: C:\Users\Ferris\AppData\Local\Programs\Python\Python312\python.exe
          0.018135s  15ms DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.8.10, skipping probing: C:\Users\Ferris\AppData\Local\Programs\Python\Python38\python.exe
        0.020176s  18ms DEBUG uv_interpreter::virtual_env Found a virtualenv named .venv at: C:\Users\Ferris\projects\uv\.venv
        0.020873s  18ms DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.12.1, skipping probing: C:\Users\Ferris\projects\uv\.venv\Scripts\python.exe
      0.021116s DEBUG uv::commands::pip_compile Using Python 3.12.1 interpreter at C:\Users\Ferris\projects\uv\.venv\Scripts\python.exe for builds
  warning: The requested Python version 3.10 is not available; 3.12.1 will be used to build dependencies instead.
```

---

_Label `enhancement` added by @konstin on 2024-02-23 12:17_

---

_Label `windows` added by @konstin on 2024-02-23 12:17_

---

_Review requested from @MichaReiser by @konstin on 2024-02-23 12:17_

---

_Review comment by @BurntSushi on `crates/gourgeist/src/interpreter.rs`:6 on 2024-02-23 12:24_

How come this isn't needed any more?

---

_@BurntSushi approved on 2024-02-23 12:24_

Nice, love it!

---

_@MichaReiser approved on 2024-02-23 12:26_

---

_@konstin reviewed on 2024-02-23 15:05_

---

_Review comment by @konstin on `crates/gourgeist/src/interpreter.rs`:6 on 2024-02-23 15:05_

I wrote the gourgeist logic when it was still a standalone crate, now the discovery in uv is much better.

---

_Merged by @konstin on 2024-02-23 18:43_

---

_Closed by @konstin on 2024-02-23 18:43_

---

_Branch deleted on 2024-02-23 18:43_

---
