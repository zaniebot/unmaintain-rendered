---
number: 13967
title: "Git usage fails on Windows with `is not compatible with the version of Windows you're running`"
type: issue
state: open
author: zanieb
labels:
  - ci-flake
assignees: []
created_at: 2025-06-11T15:09:34Z
updated_at: 2025-06-11T15:09:34Z
url: https://github.com/astral-sh/uv/issues/13967
synced_at: 2026-01-07T13:12:18-06:00
---

# Git usage fails on Windows with `is not compatible with the version of Windows you're running`

---

_Issue opened by @zanieb on 2025-06-11 15:09_

```
thread 'export::pep_751_git_dependency' panicked at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb\library\core\src\ops\function.rs:250:5:
    Unexpected failure.
    code=1
    stderr=``````
      × Failed to download and build `uv-public-pypackage @ git+[https://github.com/astral-test/uv-public-pypackage`](https://github.com/astral-test/uv-public-pypackage%60)
      ├─▶ Git operation failed
      ├─▶ could not execute process `D:\\uv-tmp\\.tmpGQ0cjl\\bin\\git.exe init` (never executed)
      ╰─▶ This version of %1 is not compatible with the version of Windows you\'re running. Check your computer\'s system information and then contact the software publisher. (os error 216)
    ```
    ```
    command=`"D:\\uv\\target\\debug\\uv.exe" "lock" "--cache-dir" "D:\\uv-tmp\\.tmpGQ0cjl\\cache"`
    code=1
    stdout=""
    stderr=```
      × Failed to download and build `uv-public-pypackage @ git+[https://github.com/astral-test/uv-public-pypackage`](https://github.com/astral-test/uv-public-pypackage%60)
      ├─▶ Git operation failed
      ├─▶ could not execute process `D:\\uv-tmp\\.tmpGQ0cjl\\bin\\git.exe init` (never executed)
      ╰─▶ This version of %1 is not compatible with the version of Windows you\'re running. Check your computer\'s system information and then contact the software publisher. (os error 216)
    ```
```

---

_Label `ci-flake` added by @zanieb on 2025-06-11 15:09_

---
