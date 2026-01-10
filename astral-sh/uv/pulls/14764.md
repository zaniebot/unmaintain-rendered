```yaml
number: 14764
title: "Create (e.g.) `python3.13t` executables in `uv venv`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/t
created_at: 2025-07-20T21:53:32Z
updated_at: 2025-07-21T16:25:51Z
url: https://github.com/astral-sh/uv/pull/14764
synced_at: 2026-01-10T06:53:02Z
```

# Create (e.g.) `python3.13t` executables in `uv venv`

---

_Pull request opened by @charliermarsh on 2025-07-20 21:53_

## Summary

CPython's `venv` module creates these, so we should too.

On non-Windows, we add `python3.13t`.

On Windows, we add `python3.13t.exe` and `pythonw3.13t.exe` (see: https://github.com/python/cpython/blob/65d2c51c10425dcfacc0a13810d58c41240d7ff9/Lib/venv/__init__.py#L362).

Closes https://github.com/astral-sh/uv/issues/14760.


---

_Label `compatibility` added by @charliermarsh on 2025-07-20 21:53_

---

_Review requested from @zanieb by @charliermarsh on 2025-07-20 21:53_

---

_Review requested from @jtfmumm by @charliermarsh on 2025-07-20 21:53_

---

_Marked ready for review by @charliermarsh on 2025-07-20 21:54_

---

_@zanieb approved on 2025-07-21 12:35_

---

_Comment by @zanieb on 2025-07-21 12:35_

You could add test coverage via `python_install_freethreaded` if you wanted

---

_@jtfmumm approved on 2025-07-21 14:55_

---

_Comment by @jtfmumm on 2025-07-21 15:23_

Looks like for the case of symlink directories, the if branch should be:

```
        if using_minor_version_link {
            let target = scripts.join(WindowsExecutable::Python.exe(interpreter));
            create_link_to_executable(target.as_path(), executable_target.clone())
                .map_err(Error::Python)?;
            let targetw = scripts.join(WindowsExecutable::Pythonw.exe(interpreter));
            create_link_to_executable(targetw.as_path(), executable_target.clone())
                .map_err(Error::Python)?;
            if interpreter.gil_disabled() {
                let targett = scripts.join(WindowsExecutable::PythonMajorMinort.exe(interpreter));
                create_link_to_executable(targett.as_path(), executable_target.clone())
                    .map_err(Error::Python)?;
                let targetwt = scripts.join(WindowsExecutable::PythonwMajorMinort.exe(interpreter));
                create_link_to_executable(targetwt.as_path(), executable_target)
                    .map_err(Error::Python)?;
            }
```

at `crates/uv-virtualenv/src/virtualenv.rs:273`

---

_Comment by @charliermarsh on 2025-07-21 16:06_

Added a test.

---

_Merged by @charliermarsh on 2025-07-21 16:25_

---

_Closed by @charliermarsh on 2025-07-21 16:25_

---

_Branch deleted on 2025-07-21 16:25_

---
