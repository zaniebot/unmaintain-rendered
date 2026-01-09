---
number: 13853
title: uv add simplepro faild
type: issue
state: closed
author: nightzjp
labels:
  - question
assignees: []
created_at: 2025-06-05T03:46:15Z
updated_at: 2025-06-05T11:16:45Z
url: https://github.com/astral-sh/uv/issues/13853
synced_at: 2026-01-07T13:12:18-06:00
---

# uv add simplepro faild

---

_Issue opened by @nightzjp on 2025-06-05 03:46_

### Summary

$ uv add simplepro
Resolved 54 packages in 31ms
  × Failed to build `django-simpleui==2025.5.262`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit code: 1)

      [stdout]
      running bdist_wheel
      running build
      running build_py
      copying simpleui\admin.py -> build\lib\simpleui
      copying simpleui\apps.py -> build\lib\simpleui
      copying simpleui\__init__.py -> build\lib\simpleui
      running egg_info
      writing django_simpleui.egg-info\PKG-INFO
      writing dependency_links to django_simpleui.egg-info\dependency_links.txt
      writing requirements to django_simpleui.egg-info\requires.txt
      writing top-level names to django_simpleui.egg-info\top_level.txt
      reading manifest file 'django_simpleui.egg-info\SOURCES.txt'
      reading manifest template 'MANIFEST.in'
      adding license file 'LICENSE'
      writing manifest file 'django_simpleui.egg-info\SOURCES.txt'
      copying simpleui\.DS_Store -> build\lib\simpleui
      copying simpleui\__pycache__\__init__.cpython-311.pyc -> build\lib\simpleui\__pycache__
      copying simpleui\__pycache__\__init__.cpython-312.pyc -> build\lib\simpleui\__pycache__
      copying simpleui\__pycache__\__init__.cpython-37.pyc -> build\lib\simpleui\__pycache__


### Platform

Windows 11 X86.64

### Version

uv 0.6.5 (bcbcd0a1e 2025-03-06)

### Python version

3.12.1

---

_Label `bug` added by @nightzjp on 2025-06-05 03:46_

---

_Comment by @konstin on 2025-06-05 11:14_

It seems that the error message is cut off?

The build error is a problem with the package, not with uv.

---

_Label `bug` removed by @konstin on 2025-06-05 11:14_

---

_Label `question` added by @konstin on 2025-06-05 11:14_

---

_Comment by @nightzjp on 2025-06-05 11:16_

> It seems that the error message is cut off?错误消息似乎被切断了？
> 
> The build error is a problem with the package, not with uv.构建错误是包的问题，而不是 uv 的问题。

That's right, I tested it and it's the problem with this package, not with UV.

---

_Closed by @nightzjp on 2025-06-05 11:16_

---
