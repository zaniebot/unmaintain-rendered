```yaml
number: 1676
title: "Windows - installing wheel fails with `Unable to extract filename from URL`"
type: issue
state: closed
author: gaborbernat
labels: []
assignees: []
created_at: 2024-02-19T01:32:14Z
updated_at: 2024-02-19T01:33:11Z
url: https://github.com/astral-sh/uv/issues/1676
synced_at: 2026-01-12T15:58:31Z
```

# Windows - installing wheel fails with `Unable to extract filename from URL`

---

_@gaborbernat_

When trying to add Windows to the tox-uv CI in https://github.com/tox-dev/tox-uv/pull/19, I noticed I get this failure:

```
py312: 4783 W install_package> C:\hostedtoolcache\windows\Python\3.12.2\x64\Scripts\uv.exe pip install --reinstall --no-deps tox_uv@D:\a\tox-uv\tox-uv\.tox\.tmp\package\1\tox_uv-0.1.dev1+g5039064-py3-none-any.whl -v [tox\tox_env\api.py:427]
 uv::requirements::from_source source=tox_uv@D:\a\tox-uv\tox-uv\.tox\.tmp\package\1\tox_uv-0.1.dev1+g5039064-py3-none-any.whl
    0.002440s DEBUG uv_interpreter::virtual_env Found a virtualenv through VIRTUAL_ENV at: D:\a\tox-uv\tox-uv\.tox\py312\.venv
    0.002773s DEBUG uv_interpreter::interpreter Using cached markers for: \\?\D:\a\tox-uv\tox-uv\.tox\py312\.venv\Scripts\python.exe
    0.002791s DEBUG uv::commands::pip_install Using Python 3.12.2 environment at D:\a\tox-uv\tox-uv\.tox\py312\.venv\Scripts\python.exe
 uv_client::flat_index::from_entries 
 uv_resolver::resolver::solve 
      0.026058s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.12.2
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.026164s   0ms DEBUG uv_resolver::resolver Adding direct dependency: tox-uv*
error: Unable to extract filename from URL: d:\a\tox-uv\tox-uv\.tox\.tmp\package\1\tox_uv-0.1.dev1+g5039064-py3-none-any.whl
py312: 4829 C exit 2 (0.05 seconds) D:\a\tox-uv\tox-uv> C:\hostedtoolcache\windows\Python\3.12.2\x64\Scripts\uv.exe pip install --reinstall --no-deps tox_uv@D:\a\tox-uv\tox-uv\.tox\.tmp\package\1\tox_uv-0.1.dev1+g5039064-py3-none-any.whl -v pid=1848 [tox\execute\api.py:280]
```

---

_Comment by @gaborbernat on 2024-02-19 01:33_

Duplicate of https://github.com/astral-sh/uv/issues/1539

---

_Closed by @gaborbernat on 2024-02-19 01:33_

---
