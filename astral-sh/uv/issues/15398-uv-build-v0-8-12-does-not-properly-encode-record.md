```yaml
number: 15398
title: "`uv-build` v0.8.12 does not properly encode `RECORD` hashes"
type: issue
state: closed
author: jwodder
labels:
  - bug
  - build-backend
assignees: []
created_at: 2025-08-20T23:01:06Z
updated_at: 2025-08-21T08:54:22Z
url: https://github.com/astral-sh/uv/issues/15398
synced_at: 2026-01-12T16:02:09Z
```

# `uv-build` v0.8.12 does not properly encode `RECORD` hashes

---

_@jwodder_

### Summary

[The wheel spec](https://packaging.python.org/en/latest/specifications/binary-distribution-format/#signed-wheel-files) states that the second column of `*.dist-info/RECORD` files should be of the form `digestname=urlsafe_b64encode_nopad(digest)`, yet uv-build 0.8.12 (and possibly earlier versions; I didn't check) generates `RECORD`s in which the digest is encoded in hexadecimal, meaning that the wheels are technically invalid.

## MVCE

Project layout:

```
./
├── pyproject.toml
└── src/
    └── foo/
        └── __init__.py
```

`pyproject.toml`:

```toml
[build-system]
requires = ["uv_build==0.8.12"]
build-backend = "uv_build"

[project]
name = "foo"
version = "0.0.0"
```

`src/foo/__init__.py`:

```python
MAGIC = 42
```

Running `pyproject-build` in this project and then inspecting the `foo-0.0.0.dist-info/RECORD` file inside `dist/foo-0.0.0-py3-none-any.whl` shows:

```csv
foo/__init__.py,sha256=cb0038f9d61736785d49dc9c3f706af1ec36d3b00c15afa58cd8adc2aab95291,11
foo-0.0.0.dist-info/WHEEL,sha256=76443c98c0efcfdd1191eac5fa1d8223dba1c474dbd47676674a255e7ca48770,79
foo-0.0.0.dist-info/METADATA,sha256=45c072393aecffac115f6801d08a2434a90937857ba7a32357c933430ee7d7c7,47
foo-0.0.0.dist-info/RECORD,,
```

For comparison, if hatchling is used instead of uv-build, then the second column of the `foo/__init__.py` line in the `RECORD` is instead `sha256=ywA4-dYXNnhdSdycP3Bq8ew207AMFa-ljNitwqq5UpE`.

### Platform

Darwin 23.6.0 x86_64

### Version

uv-build 0.8.12

### Python version

3.13.7

---

_Label `bug` added by @jwodder on 2025-08-20 23:01_

---

_Comment by @charliermarsh on 2025-08-20 23:07_

\cc @konstin 

---

_Label `build-backend` added by @zanieb on 2025-08-20 23:07_

---

_Closed by @konstin on 2025-08-21 08:54_

---
