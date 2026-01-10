```yaml
number: 3161
title: Add support for embedded Python on Windows
type: pull_request
state: merged
author: charliermarsh
labels:
  - windows
  - compatibility
assignees: []
merged: true
base: main
head: charlie/embed
created_at: 2024-04-20T15:52:41Z
updated_at: 2024-04-22T21:04:02Z
url: https://github.com/astral-sh/uv/pull/3161
synced_at: 2026-01-10T14:43:31Z
```

# Add support for embedded Python on Windows

---

_Pull request opened by @charliermarsh on 2024-04-20 15:52_

## Summary

References:
- https://github.com/pypa/virtualenv/blob/cad550030ae77e181a1d7c328742a97f2880ef9b/src/virtualenv/create/via_global_ref/builtin/cpython/cpython3.py#L58-L68
- https://github.com/pypa/virtualenv/pull/2353
- https://github.com/pypa/virtualenv/issues/2368

Closes https://github.com/astral-sh/uv/issues/1656.


---

_Label `windows` added by @charliermarsh on 2024-04-20 15:52_

---

_Label `compatibility` added by @charliermarsh on 2024-04-20 15:52_

---

_Review requested from @konstin by @charliermarsh on 2024-04-20 21:57_

---

_Marked ready for review by @charliermarsh on 2024-04-20 21:57_

---

_@charliermarsh reviewed on 2024-04-20 21:58_

---

_Review comment by @charliermarsh on `crates/uv-virtualenv/src/bare.rs`:285 on 2024-04-20 21:58_

Virtualenv is stricter on what it matches here, but I don't see other `.zip` files anyway?

---

_Review comment by @charliermarsh on `crates/uv-virtualenv/src/bare.rs`:148 on 2024-04-20 21:59_

(Just moved this up because I need it for symlinking.)

---

_@charliermarsh reviewed on 2024-04-20 21:59_

---

_Review comment by @konstin on `crates/uv-virtualenv/src/bare.rs`:227 on 2024-04-22 12:27_

nit: can we de-nest this?

---

_Review comment by @konstin on `crates/uv-virtualenv/src/bare.rs`:225 on 2024-04-22 13:20_

I think this should be a custom variant

---

_@konstin approved on 2024-04-22 13:24_

---

_@charliermarsh reviewed on 2024-04-22 16:28_

---

_Review comment by @charliermarsh on `crates/uv-virtualenv/src/bare.rs`:227 on 2024-04-22 16:28_

Like, de-nest the entire `if` hierarchy? Or de-nest the error message in some way?

---

_@konstin reviewed on 2024-04-22 16:42_

---

_Review comment by @konstin on `crates/uv-virtualenv/src/bare.rs`:227 on 2024-04-22 16:42_

Either way that doesn't get us to indentation level 14, i tried and denesting the entire thing seems the easiest

```rust
    // No symlinking on Windows, at least not on a regular non-dev non-admin Windows install.
    if cfg!(windows) {
        copy_launcher_windows(interpreter, &base_python, &scripts, python_home)?;
    }
```

```rust
/// <https://github.com/python/cpython/blob/d457345bbc6414db0443819290b04a9a4333313d/Lib/venv/__init__.py#L261-L267>
/// <https://github.com/pypa/virtualenv/blob/d9fdf48d69f0d0ca56140cf0381edbb5d6fe09f5/src/virtualenv/create/via_global_ref/builtin/cpython/cpython3.py#L78-L83>
/// There's two kinds of applications on windows: Those that allocate a console (python.exe)
/// and those that don't because they use window(s) (pythonw.exe).
fn copy_launcher_windows(
    interpreter: &Interpreter,
    base_python: &Path,
    scripts: &Path,
    python_home: &Path,
) -> Result<(), Error> {
    // First priority: the `python.exe` and `pythonw.exe` shims.
    for python_exe in ["python.exe", "pythonw.exe"] {
        let shim = interpreter
            .stdlib()
            .join("venv")
            .join("scripts")
            .join("nt")
            .join(python_exe);
        match fs_err::copy(shim, scripts.join(python_exe)) {
            Ok(_) => return Ok(()),
            Err(err) if err.kind() == io::ErrorKind::NotFound => {}
            Err(err) => {
                return Err(err.into());
            }
        }

        // Second priority: the `venvlauncher.exe` and `venvwlauncher.exe` shims.
        // These are equivalent to the `python.exe` and `pythonw.exe` shims, which were
        // renamed in Python 3.13.
        let launcher = match python_exe {
            "python.exe" => "venvlauncher.exe",
            "pythonw.exe" => "venvwlauncher.exe",
            _ => unreachable!(),
        };
        let shim = interpreter
            .stdlib()
            .join("venv")
            .join("scripts")
            .join("nt")
            .join(launcher);

        match fs_err::copy(shim, scripts.join(python_exe)) {
            Ok(_) => return Ok(()),
            Err(err) if err.kind() == io::ErrorKind::NotFound => {}
            Err(err) => {
                return Err(err.into());
            }
        }

        // Third priority: on Conda at least, we can look for the launcher shim next to
        // the Python executable itself.
        let shim = base_python.with_file_name(launcher);
        match fs_err::copy(shim, scripts.join(python_exe)) {
            Ok(_) => {}
            Err(err) if err.kind() == io::ErrorKind::NotFound => {}
            Err(err) => {
                return Err(err.into());
            }
        }

        // Fourth priority: if the launcher shim doesn't exist, assume this is
        // an embedded Python. Copy the Python executable itself, along with
        // the DLLs, `.pyd` files, and `.zip` files in the same directory.
        match fs_err::copy(
            base_python.with_file_name(python_exe),
            scripts.join(python_exe),
        ) {
            Ok(_) => {}
            Err(err) if err.kind() == io::ErrorKind::NotFound => {
                return Err(Error::IO(io::Error::new(
                    io::ErrorKind::NotFound,
                    format!(
                        "Could not find a suitable Python executable for the virtual environment (tried: {}, {}, {}, {})",
                        interpreter
                            .stdlib()
                            .join("venv")
                            .join("scripts")
                            .join("nt")
                            .join(python_exe)
                            .user_display(),
                        interpreter
                            .stdlib()
                            .join("venv")
                            .join("scripts")
                            .join("nt")
                            .join(launcher)
                            .user_display(),
                        base_python.with_file_name(launcher).user_display(),
                        base_python
                            .with_file_name(python_exe)
                            .user_display(),
                    )
                )));
            }
            Err(err) => {
                return Err(err.into());
            }
        }

        // Copy `.dll` and `.pyd` files from the top-level, and from the
        // `DLLs` subdirectory (if it exists).
        for directory in [
            python_home,
            interpreter.base_prefix().join("DLLs").as_path(),
        ] {
            let entries = match fs_err::read_dir(directory) {
                Ok(read_dir) => read_dir,
                Err(err) if err.kind() == io::ErrorKind::NotFound => {
                    continue;
                }
                Err(err) => {
                    return Err(err.into());
                }
            };
            for entry in entries {
                let entry = entry?;
                let path = entry.path();
                if path.extension().is_some_and(|ext| {
                    ext.eq_ignore_ascii_case("dll") || ext.eq_ignore_ascii_case("pyd")
                }) {
                    if let Some(file_name) = path.file_name() {
                        fs_err::copy(&path, scripts.join(file_name))?;
                    }
                }
            }
        }

        // Copy `.zip` files from the top-level.
        let entries = match fs_err::read_dir(python_home) {
            Ok(read_dir) => read_dir,
            Err(err) if err.kind() == io::ErrorKind::NotFound => {
                continue;
            }
            Err(err) => {
                return Err(err.into());
            }
        };

        for entry in entries {
            let entry = entry?;
            let path = entry.path();
            if path
                .extension()
                .is_some_and(|ext| ext.eq_ignore_ascii_case("zip"))
            {
                if let Some(file_name) = path.file_name() {
                    fs_err::copy(&path, scripts.join(file_name))?;
                }
            }
        }
    }
    Ok(())
}
```

---

_@charliermarsh reviewed on 2024-04-22 16:54_

---

_Review comment by @charliermarsh on `crates/uv-virtualenv/src/bare.rs`:227 on 2024-04-22 16:54_

Yup seems good! Will do. I assumed this is what you meant.

---

_Merged by @charliermarsh on 2024-04-22 17:34_

---

_Closed by @charliermarsh on 2024-04-22 17:34_

---

_Branch deleted on 2024-04-22 17:34_

---

_Comment by @Pixel-Minions on 2024-04-22 21:04_

Hi @charliermarsh It works great! Thank you. 



---
