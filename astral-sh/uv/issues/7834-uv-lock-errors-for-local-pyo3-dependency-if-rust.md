```yaml
number: 7834
title: "`uv lock` errors for local pyo3 dependency if Rust is not installed"
type: issue
state: closed
author: monosans
labels:
  - question
assignees: []
created_at: 2024-10-01T11:53:57Z
updated_at: 2024-10-01T13:11:41Z
url: https://github.com/astral-sh/uv/issues/7834
synced_at: 2026-01-12T15:59:17Z
```

# `uv lock` errors for local pyo3 dependency if Rust is not installed

---

_@monosans_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Output:

```bash
> uv lock --upgrade
error: Failed to build: `pyo3-example @ file:///home/user/uv-pyo3-issue/pyo3_example`
  Caused by: Build backend failed to determine metadata through `prepare_metadata_for_build_wheel` with exit status: 1
--- stdout:
Checking for Rust toolchain....
--- stderr:

Cargo, the Rust package manager, is not installed or is not on PATH.
This package requires Rust and Cargo to compile extensions. Install it through
the system's package manager or via https://rustup.rs/

---
```

Minimal example: <https://github.com/monosans/uv-pyo3-issue>

```bash
> uv --version
uv 0.4.17
```

I tried to add `name`, `version` and `requires-python` to the `pyproject.toml` of the Rust package, but it didn't help.

---

_Comment by @zanieb on 2024-10-01 12:47_

How do you expect the local dependency to be built? It sounds like it needs a Rust toolchain to build.

---

_Label `question` added by @zanieb on 2024-10-01 12:47_

---

_Comment by @charliermarsh on 2024-10-01 12:51_

(This is a failure in building your own package.)

---

_Comment by @monosans on 2024-10-01 12:55_

> How do you expect the local dependency to be built? It sounds like it needs a Rust toolchain to build.

I don't want to build it, only update the lock file

---

_Comment by @charliermarsh on 2024-10-01 12:57_

Your [`pyproject.toml`](https://github.com/monosans/uv-pyo3-issue/blob/main/pyo3_example/pyproject.toml) does not define static metadata for the package. So by rule we need to ask the build backend for the metadata. Which requires invoking `prepare_metadata_for_build_wheel`. You can get around this by defining metadata in the pyproject.toml:

```toml
[project]
name = "pyo3_example"
version = "0.0.1"
dependencies = []

[build-system]
requires = ["maturin>=1,<2"]
build-backend = "maturin"
```

---

_Comment by @charliermarsh on 2024-10-01 12:58_

```diff
diff --git a/pyo3_example/pyproject.toml b/pyo3_example/pyproject.toml
index f6f354a..256f3bd 100644
--- a/pyo3_example/pyproject.toml
+++ b/pyo3_example/pyproject.toml
@@ -1,3 +1,8 @@
+[project]
+name = "pyo3_example"
+version = "0.0.1"
+dependencies = []
+
 [build-system]
 requires = ["maturin>=1,<2"]
 build-backend = "maturin"
```

---

_Comment by @monosans on 2024-10-01 13:09_

> Your [`pyproject.toml`](https://github.com/monosans/uv-pyo3-issue/blob/main/pyo3_example/pyproject.toml?rgh-link-date=2024-10-01T12%3A57%3A55Z) does not define static metadata for the package.

It works now! Thank you very much for your work and incredible projects!


---

_Closed by @monosans on 2024-10-01 13:09_

---
