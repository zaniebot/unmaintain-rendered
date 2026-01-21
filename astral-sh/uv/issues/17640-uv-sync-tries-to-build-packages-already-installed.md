```yaml
number: 17640
title: uv sync tries to build packages already installed, and fails
type: issue
state: open
author: gaiazaff
labels:
  - bug
assignees: []
created_at: 2026-01-21T14:16:11Z
updated_at: 2026-01-21T14:16:11Z
url: https://github.com/astral-sh/uv/issues/17640
synced_at: 2026-01-21T15:05:52Z
```

# uv sync tries to build packages already installed, and fails

---

_@gaiazaff_

### Summary

I get an error installing some packages during a normal sync.
I get this here for `antlr4-python3-runtime` v4.9.3, but it also happens for `pylatexenc==2.10` if I do `uv sync --upgrade`.

```
$ source .venv/Scripts/activate

$ uv sync
Resolved 277 packages in 8ms
  × Failed to download and build `antlr4-python3-runtime==4.9.3`                                                                                                                  
  ├─▶ Failed to install requirements from `build-system.requires`                                                                                                                 
  ├─▶ Failed to download `setuptools==80.10.1`                                                                                                                                    
  ├─▶ Failed to extract archive: setuptools-80.10.1-py3-none-any.whl                                                                                                              
  ╰─▶ unexpected BufError                                                                                                                                                         
  help: `antlr4-python3-runtime` (v4.9.3) was included because [mypackage] (v0.2.2) depends on `docling` (v2.69.0) which depends on `rapidocr` (v3.5.0) which  
        depends on `omegaconf` (v2.3.0) which depends on `antlr4-python3-runtime`                                                                                                                                                                                                                                                           
 ```
to verify what the issue was, I installed the package with pip:
```
$ pip install antlr4-python3-runtime                                                                                                                                              
Collecting antlr4-python3-runtime                                                                                                                                                 
  Downloading antlr4_python3_runtime-4.13.2-py3-none-any.whl.metadata (304 bytes)                                                                                                 
Downloading antlr4_python3_runtime-4.13.2-py3-none-any.whl (144 kB)                                                                                                               
Installing collected packages: antlr4-python3-runtime                                                                                                                             
Successfully installed antlr4-python3-runtime-4.13.2                                                                                                                              
```
it worked without issues.
I even installed setuptools==80.10.1 with pip, without issues.

But still uv doesn't consider it a resolved requirement:
```
$ uv sync                                                                                                                                                                         
Resolved 277 packages in 8ms                                                                                                                                                      
  × Failed to download and build `antlr4-python3-runtime==4.9.3`
  ├─▶ Failed to install requirements from `build-system.requires`
  ├─▶ Failed to download `setuptools==80.10.1`
  ├─▶ Failed to extract archive: setuptools-80.10.1-py3-none-any.whl
  ╰─▶ unexpected BufError
  help: `antlr4-python3-runtime` (v4.9.3) was included because[mypackage] (v0.2.2) depends on `docling` (v2.69.0) which depends on `rapidocr` (v3.5.0) which  
        depends on `omegaconf` (v2.3.0) which depends on `antlr4-python3-runtime`
```

I already have antlr4-python3-runtime>4.9.3 actually, and setuptools==80.10.1, and pylatexenc==2.10 in my venv, so none of them are needed!



### Platform

Windows 11 x86_64

### Version

0.9.26 (ee4f00362 2026-01-15)

### Python version

3.12.9

---

_Label `bug` added by @gaiazaff on 2026-01-21 14:16_

---
