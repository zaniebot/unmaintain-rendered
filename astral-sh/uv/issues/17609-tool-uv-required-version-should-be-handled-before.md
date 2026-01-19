```yaml
number: 17609
title: tool.uv.required-version should be handled before parsing rest of pyproject.toml file
type: issue
state: open
author: fellhorn
labels:
  - bug
assignees: []
created_at: 2026-01-19T10:16:44Z
updated_at: 2026-01-19T10:17:17Z
url: https://github.com/astral-sh/uv/issues/17609
synced_at: 2026-01-19T10:28:48Z
```

# tool.uv.required-version should be handled before parsing rest of pyproject.toml file

---

_@fellhorn_

### Summary

`tool.uv.required-version` (#8605) allows specifying uv version constraints in the `pyproject.toml` file. uv commands should fail if the user's uv version does not match the constraint.

But parsing the `pyproject.toml` file can fail if it uses settings that are not yet present in the user's version of uv. In this case uv will only show a warning instead and ignore the version mismatch:

 ```bash
warning: Failed to parse `pyproject.toml` during settings discovery:
```

## Example

`pyproject.toml` file with a setting introduced in `0.9.25`:

```toml
[tool.uv]
required-version = ">=0.9.26"

[tool.uv.exclude-newer-package]
numpy = false
````

Running then e.g. `uv pip install numpy` with uv version `0.9.24` will show this warning but still continue:

```bash
warning: Failed to parse `pyproject.toml` during settings discovery:
TOML parse error at line 5, column 9
|
5 | numpy = false
|         ^^^^^
data did not match any variant of untagged enum Helper

Using Python 3.11.9 environment at: /home/fellhorn/somewhere/somewhere
Audited 1 package in 5ms
```

But running it with uv `0.9.25` will throw an error and exit:

```bash
error: Required uv version `>=0.9.26` does not match the running version `0.9.25`. Update `uv` by running `uv self update`.
```

### Platform

Linux 6.8.0-90-generic x86_64 GNU/Linux

### Version

uv 0.9.24

### Python version

_No response_

---

_Label `bug` added by @fellhorn on 2026-01-19 10:16_

---
