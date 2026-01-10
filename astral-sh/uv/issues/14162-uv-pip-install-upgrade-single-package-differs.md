---
number: 14162
title: "\"uv pip install\" upgrade single package differs from pip"
type: issue
state: closed
author: ewohnlich
labels:
  - question
assignees: []
created_at: 2025-06-20T15:48:09Z
updated_at: 2025-06-20T16:30:20Z
url: https://github.com/astral-sh/uv/issues/14162
synced_at: 2026-01-10T01:25:43Z
---

# "uv pip install" upgrade single package differs from pip

---

_Issue opened by @ewohnlich on 2025-06-20 15:48_

### Question

If I want to upgrade a single package (and resolve any deps changes) in pip I would do "pip install --upgrade collective.z3cform.datagridfield", as an example. That package requires CMFPlone but does not pin a specific version, so CMFPlone is not updated. However with uv, "uv pip install --upgrade collective.z3cform.datagridfield" it _does_ upgrade CMFPlone, which I do not want. If I specify a version for the package it will only update that package "uv pip install collective.z3cform.datagridfield==3.0.4" but I would like to be able to get the latest version without knowing what it is. What command should I be using?

As a proof of concept, I created a pip venv and a uv venv to document the discrepancy.

pip
```
PS C:\Users\wohnlice> & 'C:\Program Files\Python311\python.exe' -m venv vtest1
PS C:\Users\wohnlice> cd .\vtest1\
PS C:\Users\wohnlice\vtest1> .\Scripts\activate
(vtest1) PS C:\Users\wohnlice\vtest1> python -m pip install --upgrade pip
...
(vtest1) PS C:\Users\wohnlice\vtest1> pip install plone==6.0.14 -c https://dist.plone.org/release/6.0.14/constraints.txt
...
(vtest1) PS C:\Users\wohnlice\vtest1> pip list | FINDSTR CMFPlone
Products.CMFPlone                   6.0.14
(vtest1) PS C:\Users\wohnlice\vtest1> pip install collective.z3cform.datagridfield==3.0.0
...
(vtest1) PS C:\Users\wohnlice\vtest1> pip list | FINDSTR CMFPlone
Products.CMFPlone                   6.0.14
(vtest1) PS C:\Users\wohnlice\vtest1> pip install --upgrade collective.z3cform.datagridfield
...
(vtest1) PS C:\Users\wohnlice\vtest1> pip list | FINDSTR datagrid
collective.z3cform.datagridfield    3.0.4
(vtest1) PS C:\Users\wohnlice\vtest1> pip list | FINDSTR CMFPlone
Products.CMFPlone                   6.0.14
```

uv
```
(Plone61) PS C:\Users\wohnlice> uv venv vtest2
Using CPython 3.11.9 interpreter at: C:\Program Files\Python311\python.exe
Creating virtual environment at: vtest2
Activate with: vtest2\Scripts\activate
(Plone61) PS C:\Users\wohnlice> .\vtest2\Scripts\activate
(vtest2) PS C:\Users\wohnlice> uv pip install plone==6.0.14 -c https://dist.plone.org/release/6.0.14/constraints.txt
...
(vtest2) PS C:\Users\wohnlice>uv pip list | FINDSTR cmfplone
Using Python 3.11.9 environment at: vtest2
products-cmfplone              6.0.14
(vtest2) PS C:\Users\wohnlice> uv pip install collective.z3cform.datagridfield==3.0.0
(vtest2) PS C:\Users\wohnlice>uv pip list | FINDSTR cmfplone
Using Python 3.11.9 environment at: vtest2
products-cmfplone              6.0.14
(vtest2) PS C:\Users\wohnlice> uv pip install --upgrade collective.z3cform.datagridfield
...
(vtest2) PS C:\Users\wohnlice> uv pip list | FINDSTR cmfplone
Using Python 3.11.9 environment at: vtest2
products-cmfplone                6.1.2
```

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @ewohnlich on 2025-06-20 15:48_

---

_Comment by @charliermarsh on 2025-06-20 16:30_

I believe this is a duplicate of https://github.com/astral-sh/uv/issues/4779.

---

_Closed by @charliermarsh on 2025-06-20 16:30_

---
