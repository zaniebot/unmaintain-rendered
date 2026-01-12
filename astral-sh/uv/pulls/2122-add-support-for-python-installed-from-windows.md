```yaml
number: 2122
title: Add support for Python installed from Windows Store
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/win
created_at: 2024-03-01T21:21:25Z
updated_at: 2024-03-03T17:55:37Z
url: https://github.com/astral-sh/uv/pull/2122
synced_at: 2026-01-12T16:04:52Z
```

# Add support for Python installed from Windows Store

---

_@charliermarsh_

## Summary

After https://github.com/astral-sh/uv/pull/2121, the only remaining issue is that calling `canonicalize` on these Pythons returns an error. 

Closes https://github.com/astral-sh/uv/issues/2105.

## Test Plan

Uninstalled all python.org Pythons on my Windows machine, then created a virtualenv. The resulting config file:

```
Using Python 3.11.8 interpreter at: C:\Users\crmar\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\python.exe
Creating virtualenv at: .venv
Activate with: .venv\Scripts\activate
PS C:\Users\crmar\workspace\puffin> cat .\.venv\pyvenv.cfg
home = C:\Users\crmar\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0
implementation = CPython
version_info = 3.11.8
include-system-site-packages = false
uv = 0.1.13
prompt = puffin
```

Prior to this PR, it would fail with a canonicalization error.

Prior to #2121, it would leave a "bad" Python in the config file:

```
Using Python 3.11.8 interpreter at: C:\Users\crmar\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\python.exe
Creating virtualenv at: .venv
Activate with: .venv\Scripts\activate
PS C:\Users\crmar\workspace\puffin> cat .\.venv\pyvenv.cfg
home = C:\Program Files\WindowsApps\PythonSoftwareFoundation.Python.3.11_3.11.2288.0_x64__qbz5n2kfra8p0
implementation = CPython
version_info = 3.11.8
include-system-site-packages = false
uv = 0.1.13
prompt = puffin
```

Which, once activated, would fail with:

```
(venv) PS C:\Users\crmar\workspace\puffin> python
No Python at '"C:\Users\crmar\AppData\Local\Programs\Python\Python312\python.exe'
```



---

_Review requested from @konstin by @charliermarsh on 2024-03-01 21:21_

---

_Label `bug` added by @charliermarsh on 2024-03-01 21:21_

---

_Label `windows` added by @charliermarsh on 2024-03-01 21:21_

---

_Comment by @charliermarsh on 2024-03-01 21:22_

For posterity, in early iterations of this PR I had some code like:

```rust
/// Given a path, return the "real" path.
///
/// The only modifications applied here are for the Windows Store. Specifically, the Windows Store
/// Python executable is located at a path like:
///
///     `C:\Program Files\WindowsApps\PythonSoftwareFoundation.Python.3.11_qbs5n2kfra8p0`.
///
/// However, users actually aren't allowed to run executables from this path directly. So, instead,
/// we need to use a path like:
///
///     `C:\Users\crmar\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.11_qbs5n2kfra8p0`
///
/// In practice, this is achieved by replacing `%ProgramFiles%\WindowsApps` with `%LocalAppData%\Microsoft\WindowsApps`.
///
/// See: <https://github.com/python-poetry/poetry/pull/5931>
fn real_path(path: &Path) -> io::Result<PathBuf> {
    #[cfg(windows)]
    if path.is_absolute() {
        if path.components().any(|component| component.as_os_str() == "Microsoft") {
            return real_windows_path(path);
        }
    }

    Ok(path.to_path_buf())
}

#[cfg(windows)]
fn real_windows_path(path: &Path) -> io::Result<PathBuf> {
    use windows_sys::Win32::UI::Shell::{CSIDL_LOCAL_APPDATA, CSIDL_PROFILE};

    let program_files = windows_folder(CSIDL_PROGRAM_FILES)?;
    let local_appdata = windows_folder(CSIDL_LOCAL_APPDATA)?;

    let suffix = path.strip_prefix(&program_files)?;
    Ok(local_appdata.join(suffix))
}

#[cfg(windows)]
fn windows_folder(csidl: u32) -> Option<PathBuf> {
    use std::env;
    use std::ffi::OsString;
    use std::os::windows::ffi::OsStringExt;
    use std::path::PathBuf;

    use windows_sys::Win32::Foundation::{MAX_PATH, S_OK};
    use windows_sys::Win32::UI::Shell::{SHGetFolderPathW};

    unsafe {
        let mut path: Vec<u16> = Vec::with_capacity(MAX_PATH as usize);
        match SHGetFolderPathW(0, csidl as i32, 0, 0, path.as_mut_ptr()) {
            S_OK => {
                let len = wcslen(path.as_ptr());
                path.set_len(len);
                let s = OsString::from_wide(&path);
                Some(PathBuf::from(s))
            }
            _ => None,
        }
    }
}

#[cfg(windows)]
extern "C" {
    fn wcslen(buf: *const u16) -> usize;
}
```

But this ended up not being necessary or sufficient, using `sys._base_executable` did the thing I needed.

---

_Review comment by @konstin on `crates/uv-fs/src/path.rs`:102 on 2024-03-03 17:40_

nit: I'm a fan of making these `if !cfg!(windows) || !path.is_absolute() { return false; }` and getting rid of the nesting

---

_@konstin approved on 2024-03-03 17:42_

---

_Merged by @charliermarsh on 2024-03-03 17:55_

---

_Closed by @charliermarsh on 2024-03-03 17:55_

---

_Branch deleted on 2024-03-03 17:55_

---
