```yaml
number: 14438
title: "`uv build` failed to build"
type: issue
state: closed
author: chirizxc
labels:
  - bug
assignees: []
created_at: 2025-07-03T10:57:05Z
updated_at: 2025-07-09T17:45:47Z
url: https://github.com/astral-sh/uv/issues/14438
synced_at: 2026-01-10T03:32:45Z
```

# `uv build` failed to build

---

_Issue opened by @chirizxc on 2025-07-03 10:57_

### Summary

```bash
❯ uv build                                                                                                                                            
Building source distribution (uv build backend)...                                                                                                    
Building wheel from source distribution (uv build backend)...
  x Failed to build `C:\Users\chiri\Desktop\tst`
  |-> Failed to walk source tree: `C:\Users\chiri\AppData\Local\uv\cache\sdists-v9\.tmptJQX7u\tst-0.1.0`
  |-> IO error for operation on C:\Users\chiri\AppData\Local\uv\cache\sdists-v9\.tmptJQX7u\tst-0.1.0\src\tst: Не удается найти указанный файл. (os
  |   error 2)
  `-> Не удается найти указанный файл. (os error 2)
```

```toml
[build-system]
requires = ["uv_build>=0.7.19,<0.8.0"]
build-backend = "uv_build"

[tool.uv.build-backend]
namespace = true

[tool.uv.workspace]
members = [
    "test_1",
    "test_2",
]

[project]
name = "tst"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13.0"
dependencies = [
    "adaptix>=3.0.0b11,<4.0.0",
]
```

![Image](https://github.com/user-attachments/assets/fed7bad0-82f8-4743-a7d5-c43e466b1d40)

### Platform

Windows 10

### Version

uv 0.7.19 (38ee6ec80 2025-07-02) 

### Python version

Python 3.13.0   

---

_Label `bug` added by @chirizxc on 2025-07-03 10:57_

---

_Comment by @chirizxc on 2025-07-03 11:20_

oh, i guess i forgot to make the `tst` folder a level higher 

---

_Closed by @chirizxc on 2025-07-03 11:20_

---

_Reopened by @chirizxc on 2025-07-03 11:32_

---

_Comment by @chirizxc on 2025-07-03 11:34_

if we want to have several modules in `src`, then at:
```toml
[tool.uv.build-backend]
namespace = true
```
I think it makes sense to override `module-name` to `module-name = ""`, if it's not in `pyproject.toml`

because: 
```toml
[tool.uv.build-backend]
module-name = ""
namespace = true

[tool.uv.workspace]
members = [
    "test_1",
    "test_2",
]
```
=>
```bash
❯ uv build                                                                                                                                            
Building source distribution (uv build backend)...                                                                                                    
Building wheel from source distribution (uv build backend)...
Successfully built dist\tst-0.1.0.tar.gz
Successfully built dist\tst-0.1.0-py3-none-any.whl
```

and

```diff
[tool.uv.build-backend]
- module-name = ""
namespace = true

[tool.uv.workspace]
members = [
    "test_1",
    "test_2",
]
```
=>
```bash
Building source distribution (uv build backend)...                                                                                                    
Building wheel from source distribution (uv build backend)...
  x Failed to build `C:\Users\chiri\Desktop\tst`
  |-> Failed to walk source tree: `C:\Users\chiri\AppData\Local\uv\cache\sdists-v9\.tmpQgy9eH\tst-0.1.0`
  |-> IO error for operation on C:\Users\chiri\AppData\Local\uv\cache\sdists-v9\.tmpQgy9eH\tst-0.1.0\src\tst: Не удается найти указанный файл. (os    
  |   error 2)
  `-> Не удается найти указанный файл. (os error 2)
```

---

_Comment by @chirizxc on 2025-07-03 11:52_

https://github.com/astral-sh/uv/blob/main/crates/uv-build-backend/src/lib.rs#L216

should probably do something like:
```rust
let module_relative = match module_name {
 Some(name) if !name.trim().is_empty() => name.split('.').collect::<PathBuf>(),
 _ => PathBuf::new(),
};
````

---

_Assigned to @konstin by @zanieb on 2025-07-03 11:53_

---

_Comment by @zanieb on 2025-07-03 11:53_

Thanks for the report!

---

_Closed by @zanieb on 2025-07-09 17:45_

---
