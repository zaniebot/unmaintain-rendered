```yaml
number: 2506
title: "The `--force-exclude` flag is behaving differently to `ruff`"
type: issue
state: open
author: btalb
labels:
  - bug
  - cli
assignees: []
created_at: 2026-01-15T07:08:50Z
updated_at: 2026-01-15T09:01:15Z
url: https://github.com/astral-sh/ty/issues/2506
synced_at: 2026-01-15T09:45:23Z
```

# The `--force-exclude` flag is behaving differently to `ruff`

---

_@btalb_

### Summary

Following on from https://github.com/astral-sh/ty/issues/1469, I expect the behaviour of `--force-exclude` to be the same for `ruff check` and `ty check` But what I'm seeing seems to differ (`ty` doesn't seem to obey the flag at all?).

Below is an MVP where I run them both in a project with a file passed via CLI:

```
bt@FA-02090-LNX:~/repos/autonomy/DEV-3484-ruff-with-shebangs$ uvx ruff --version
ruff 0.14.11
bt@FA-02090-LNX:~/repos/autonomy/DEV-3484-ruff-with-shebangs$ uvx ty --version
ty 0.0.12
bt@FA-02090-LNX:~/repos/autonomy/DEV-3484-ruff-with-shebangs$ uvx ruff check --force-exclude -v out/amd64/install/_setup_util.py 
[2026-01-15][16:56:48][ruff::resolve][DEBUG] Using configuration file (via parent) at: /home/bt/repos/autonomy/DEV-3484-ruff-with-shebangs/pyproject.toml
[2026-01-15][16:56:48][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/home/bt/repos/autonomy/DEV-3484-ruff-with-shebangs/out"
[2026-01-15][16:56:48][ruff::commands::check][DEBUG] Identified files to lint in: 50.209µs
warning: No Python files found under the given path(s)
All checks passed!
bt@FA-02090-LNX:~/repos/autonomy/DEV-3484-ruff-with-shebangs$ uvx ty check --force-exclude -v out/amd64/install/_setup_util.py 
INFO Defaulting to python-platform `linux`
INFO Python version: Python 3.12, platform: linux
INFO Indexed 1 file(s) in 0.002s
error[invalid-argument-type]: Argument to bound method `insert` is incorrect
   --> out/amd64/install/_setup_util.py:283:41
    |
282 |         if base_path not in CMAKE_PREFIX_PATH:
283 |             CMAKE_PREFIX_PATH.insert(0, base_path)
    |                                         ^^^^^^^^^ Expected `LiteralString`, found `str`
284 |         CMAKE_PREFIX_PATH = os.pathsep.join(CMAKE_PREFIX_PATH)
    |
info: Method defined here
    --> stdlib/builtins.pyi:2865:9
     |
2863 |         """Return number of occurrences of value."""
2864 |
2865 |     def insert(self, index: SupportsIndex, object: _T, /) -> None:
     |         ^^^^^^                             ---------- Parameter declared here
2866 |         """Insert object before index."""
     |
info: Union variant `bound method list[LiteralString].insert(index: SupportsIndex, object: LiteralString, /) -> None` is incompatible with this call site
info: Attempted to call union type `(bound method list[LiteralString].insert(index: SupportsIndex, object: LiteralString, /) -> None) | (bound method list[Unknown].insert(index: SupportsIndex, object: Unknown, /) -> None)`
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

And I've confirmed that when running without a file passed, `ty`'s use of `pyproject.toml` correctly excludes it:
```
bt@FA-02090-LNX:~/repos/autonomy/DEV-3484-ruff-with-shebangs$ uvx ty check -vvv |& grep ' Checking file .*_setup_util.py'
bt@FA-02090-LNX:~/repos/autonomy/DEV-3484-ruff-with-shebangs$ 
```

My excludes are specified in this part of the `pyproject.toml`:
```
[tool.ty.src]
exclude = [
  ".git",
  ".ycm_extra_conf.py",
  "docker",
  "out",
]
```

### Version

0.0.12

---

_Label `bug` added by @MichaReiser on 2026-01-15 09:00_

---

_Label `cli` added by @MichaReiser on 2026-01-15 09:00_

---

_Comment by @MichaReiser on 2026-01-15 09:01_

Thanks, this is a bug

---
