```yaml
number: 17402
title: cant activate python venv via nu shell batch
type: issue
state: open
author: PoomTheerapat
labels:
  - bug
assignees: []
created_at: 2026-01-10T19:16:39Z
updated_at: 2026-01-10T19:16:39Z
url: https://github.com/astral-sh/uv/issues/17402
synced_at: 2026-01-12T02:26:26Z
```

# cant activate python venv via nu shell batch

---

_Issue opened by @PoomTheerapat on 2026-01-10 19:16_

### Summary

~\Desktop\test> uv venv
Using CPython 3.14.2
Creating virtual environment at: .venv
Activate with: overlay use .venv/Scripts/activate.nu
~\Desktop\test> overlay use .venv/Scripts/activate.nu
Error: nu::shell::name_not_found

  × Name not found
    ╭─[C:\Users\KATANA\Desktop\test\.venv\Scripts\activate.nu:75:32]
 74 │     let venv_path = ([$virtual_env $bin] | path join)
 75 │     let new_path = ($env | get $path_name | prepend $venv_path)
    ·                                ─────┬────
    ·                                     ╰── did you mean 'path'?
 76 │     let virtual_env_prompt = if ('test' | is-empty) {
    ╰────

both uv and nu shell is latest version

### Platform

Windows11

### Version

uv 0.9.24 (0fda1525e 2026-01-09)

### Python version

Python 3.9.11

---

_Label `bug` added by @PoomTheerapat on 2026-01-10 19:16_

---
