---
number: 5078
title: Simplify union of marker expression and negation when split in disjunction
type: issue
state: closed
author: ibraheemdev
labels:
  - enhancement
  - lock
assignees: []
created_at: 2024-07-15T17:56:55Z
updated_at: 2024-08-20T20:10:52Z
url: https://github.com/astral-sh/uv/issues/5078
synced_at: 2026-01-07T13:12:17-06:00
---

# Simplify union of marker expression and negation when split in disjunction

---

_Issue opened by @ibraheemdev on 2024-07-15 17:56_

More complex case of https://github.com/astral-sh/uv/issues/5044, e.g. `(platform_machine == 'x86_64' and platform_system == 'Windows') or (platform_machine != 'x86_64' and platform_system == 'Windows')` could be simplified to `platform_system == 'Windows'`.

Shows up with:
```
--find-links https://download.pytorch.org/whl/torch_stable.html

torch==2.3.1+cpu ; platform_machine == 'x86_64'
torch==2.3.1+cu118 ; platform_machine != 'x86_64'
```
```
filelock==3.13.1
    # via torch
fsspec==2024.3.1
    # via torch
intel-openmp==2021.4.0 ; (platform_machine == 'x86_64' and platform_system == 'Windows') or (platform_machine != 'x86_64' and platform_system == 'Windows')
    # via mkl
jinja2==3.1.3
    # via torch
markupsafe==2.1.5
    # via jinja2
mkl==2021.4.0 ; (platform_machine == 'x86_64' and platform_system == 'Windows') or (platform_machine != 'x86_64' and platform_system == 'Windows')
    # via torch
mpmath==1.3.0
    # via sympy
networkx==3.2.1
    # via torch
sympy==1.12
    # via torch
tbb==2021.11.0 ; (platform_machine == 'x86_64' and platform_system == 'Windows') or (platform_machine != 'x86_64' and platform_system == 'Windows')
    # via mkl
torch==2.3.1+cpu ; platform_machine == 'x86_64'
    # via -r requirements.in
torch==2.3.1+cu118 ; platform_machine != 'x86_64'
    # via -r requirements.in
typing-extensions==4.10.0
    # via torch
```

---

_Label `enhancement` added by @ibraheemdev on 2024-07-15 17:56_

---

_Label `lock` added by @ibraheemdev on 2024-07-15 17:56_

---

_Comment by @ibraheemdev on 2024-07-15 18:01_

Possibly what we should to do here is normalize to CNF or DNF.

---

_Closed by @charliermarsh on 2024-08-20 20:10_

---
