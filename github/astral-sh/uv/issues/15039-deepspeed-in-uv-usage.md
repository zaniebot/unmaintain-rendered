---
number: 15039
title: Deepspeed in uv usage
type: issue
state: closed
author: Virtuoso461
labels:
  - bug
assignees: []
created_at: 2025-08-03T05:33:32Z
updated_at: 2025-08-26T02:24:07Z
url: https://github.com/astral-sh/uv/issues/15039
synced_at: 2026-01-07T13:12:19-06:00
---

# Deepspeed in uv usage

---

_Issue opened by @Virtuoso461 on 2025-08-03 05:33_

### Summary

PS D:\PythonSource\model> uv pip install deepspeed                                                                                                                                                                                                           
Resolved 25 packages in 20ms
  × Failed to build `deepspeed==0.17.4`
  ├─▶ The build backend returned an error                                                                                                                                                                                                                    
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit code: 1)                                                                                                                                                                                                                                                                                                                                                                                                                                              [stdout]                                                                                                                                                                                                                                               
      [WARNING] Unable to import torch, pre-compiling ops will be disabled. Please visit https://pytorch.org/ to see how to properly install torch on your system.                                                                                           
       [WARNING]  unable to import torch, please install it if you want to pre-compile any deepspeed ops.                                                                                                                                                    
      DS_BUILD_OPS=1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            [stderr]                                                                                                                                                                                                                                               
      Traceback (most recent call last):                                                                                                                                                                                                                     
        File "<string>", line 14, in <module>                                                                                                                                                                                                                
          requires = get_requires_for_build({})                                                                                                                                                                                                              
        File "C:\Users\24160\AppData\Local\uv\cache\builds-v0\.tmpQfabXB\Lib\site-packages\setuptools\build_meta.py", line 331, in get_requires_for_build_wheel                                                                                              
          return self._get_build_requires(config_settings, requirements=[])                                                                                                                                                                                  
                 ~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^                                                                                                                                                                                  
        File "C:\Users\24160\AppData\Local\uv\cache\builds-v0\.tmpQfabXB\Lib\site-packages\setuptools\build_meta.py", line 301, in _get_build_requires                                                                                                       
          self.run_setup()                                                                                                                                                                                                                                   
          ~~~~~~~~~~~~~~^^                                                                                                                                                                                                                                   
        File "C:\Users\24160\AppData\Local\uv\cache\builds-v0\.tmpQfabXB\Lib\site-packages\setuptools\build_meta.py", line 512, in run_setup                                                                                                                 
          super().run_setup(setup_script=setup_script)                                                                                                                                                                                                       
          ~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^                                                                                                                                                                                                       
        File "C:\Users\24160\AppData\Local\uv\cache\builds-v0\.tmpQfabXB\Lib\site-packages\setuptools\build_meta.py", line 317, in run_setup                                                                                                                 
          exec(code, locals())                                                                                                                                                                                                                               
          ~~~~^^^^^^^^^^^^^^^^
        File "<string>", line 157, in <module>
      AssertionError: Unable to pre-compile ops without torch installed. Please install torch before attempting to pre-compile ops.

      hint: This usually indicates a problem with the package or the build environment.
PS D:\PythonSource\model> uv pip list
Package           Version
----------------- ------------
filelock          3.13.1
fsspec            2024.6.1
jinja2            3.1.4
markupsafe        2.1.5
mpmath            1.3.0
networkx          3.3
numpy             2.1.2
pillow            11.0.0
setuptools        70.2.0
sympy             1.13.3
torch             2.7.1+cu128
torchaudio        2.7.1+cu128
torchvision       0.22.1+cu128
typing-extensions 4.12.2

### Platform

Windows11

### Version

uv 0.7.5 (9d1a14e1f 2025-05-16)

### Python version

Python 3.13.3

---

_Label `bug` added by @Virtuoso461 on 2025-08-03 05:33_

---

_Comment by @charliermarsh on 2025-08-06 20:28_

Can you upgrade to latest uv and try adding:

```toml
[tool.uv.extra-build-dependencies]
deepspeed = ["torch"]
```

To your `pyproject.toml`?

---

_Referenced in [astral-sh/uv#15118](../../astral-sh/uv/issues/15118.md) on 2025-08-06 20:54_

---

_Closed by @charliermarsh on 2025-08-26 02:24_

---
