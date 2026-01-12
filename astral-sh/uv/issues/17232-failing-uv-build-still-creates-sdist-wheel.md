```yaml
number: 17232
title: "Failing `uv build` still creates sdist/wheel"
type: issue
state: closed
author: mkniewallner
labels:
  - bug
assignees: []
created_at: 2025-12-23T23:13:09Z
updated_at: 2026-01-06T14:27:06Z
url: https://github.com/astral-sh/uv/issues/17232
synced_at: 2026-01-12T16:02:46Z
```

# Failing `uv build` still creates sdist/wheel

---

_@mkniewallner_

### Summary

When using `uv build`, if the build fails, a distribution is still created under `dist` directory.

## Minimal reproducer

Create a new directory that only contains the following `pyproject.toml`:
```toml
[project]
name = "foo"
version = "0.0.1"

[build-system]
requires = ["uv_build"]
build-backend = "uv_build"
```

```console
$ uv build
Building source distribution...
Error: Expected a Python module at: src/foo/__init__.py
  × Failed to build `<REDACTED>/foo`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `uv_build.build_sdist` failed (exit status: 1)
      hint: This usually indicates a problem with the package or the build environment.
```

The build fails as expected, but an sdist that contains `PKG-INFO` is still created:

```console
$ tar -tzf dist/foo-0.0.1.tar.gz
foo-0.0.1/PKG-INFO
```

A similar thing happens for wheels, where running `uv build --wheel` will create an empty wheel:

```console
$ uv build --wheel
Building wheel...
Error: Expected a Python module at: src/foo/__init__.py
  × Failed to build `<REDACTED>/foo`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `uv_build.build_wheel` failed (exit status: 1)
      hint: This usually indicates a problem with the package or the build environment.

$ unzip -l dist/foo-0.0.1-py3-none-any.whl
Archive:  dist/foo-0.0.1-py3-none-any.whl
warning [dist/foo-0.0.1-py3-none-any.whl]:  zipfile is empty
```

## Expected behaviour

No sdist or wheel is created when `uv build` fails. It's a minor thing TBH, but in a CI, if the return code is not properly handled, it could lead to trying to publish a faulty sdist or wheel to PyPI.

### Platform

macOS 26 arm64

### Version

uv 0.9.18 (Homebrew 2025-12-16)

### Python version

Python 3.14.2

---

_Label `bug` added by @mkniewallner on 2025-12-23 23:13_

---

_Renamed from "Failing `uv build` still create sdist/wheel" to "Failing `uv build` still creates sdist/wheel" by @mkniewallner on 2025-12-23 23:14_

---

_Comment by @ValdonVitija on 2025-12-25 20:12_

Can anyone confirm if this is the 'desired' behavior. If yes, I would give this one a try!

---

_Comment by @bybrooks on 2025-12-27 11:58_

@ValdonVitija

Hi, I'm looking to make my first OSS contribution and investigated this issue.

**Root Cause Analysis:**

In `crates/uv-build-backend/src/source_dist.rs` and `crates/uv-build-backend/src/wheel.rs`, the build process follows these steps:

1. `TarGzWriter::new()` / `File::create()` → Creates the output file **first**（[Code](https://github.com/astral-sh/uv/blob/main/crates/uv-build-backend/src/source_dist.rs#L36)）
2. Writes PKG-INFO / WHEEL metadata
3. `find_roots()` validates the Python module structure ← **Error occurs here**
4. Writes module files

The problem is that when an error occurs in step 3, the file created in step 1 is not cleaned up.

**Proposed Fix:**

Add cleanup logic in `build_source_dist()` and `build_wheel()` to remove the created file when `write_source_dist()` / `write_wheel()` returns an error:

```rs
// Example fix for build_source_dist()
let writer = TarGzWriter::new(&source_dist_path)?;
if let Err(err) = write_source_dist(source_tree, writer, uv_version, show_warnings) {
    let _ = fs_err::remove_file(&source_dist_path);  // Add cleanup
    return Err(err);
}The same fix should be applied to `build_wheel()` and `build_editable()`.
```

If this approach looks good, I'd be happy to submit a PR.

---

_Comment by @EliteTK on 2025-12-29 15:46_

@bybrooks I believe the more robust approach here would be to use a [named temporary file](https://docs.rs/tempfile/latest/tempfile/struct.NamedTempFile.html) and persist it once we're certain we have succeeded in writing it. You will need to use `new_in` to put it in the right directory (you don't want it in the normal temp location because then you can't atomically rename it (or rename it at all really) if it happens to be on a different filesystem).

---

_Closed by @konstin on 2026-01-06 14:27_

---
