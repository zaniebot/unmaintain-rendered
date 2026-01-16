```yaml
number: 17504
title: editable dependency and name binding
type: issue
state: open
author: vsashidh
labels:
  - question
assignees: []
created_at: 2026-01-15T22:36:03Z
updated_at: 2026-01-15T23:05:57Z
url: https://github.com/astral-sh/uv/issues/17504
synced_at: 2026-01-16T00:03:13Z
```

# editable dependency and name binding

---

_@vsashidh_

### Question

I have the following hierarchy init using a library `--lib`:
```
myLibProj/
                src/
                   mylibproj/
                             __init__.py
                             foo.py
                              
```
and in the `__init__.py`, i have the following import to bind a class attribute in the `foo.py` submodule with the following statement:
`from .foo import FooClass`

That way I can import in other modules using `from mylibproj import FooClass` instead of `from mylibproj.foo import FooClass`. 

When I use this library project in another project as an editable dependency, I'm unable to import FooClass from the package directly. And it requires me to import the standard way: `from mylibproj.foo import FooClass`. The specific error depicted in Pylance is `"FooClass" is not exported from module mylibproj`.

Is this import behavior expected?

### Platform

Windows 11 x86_64

### Version

_No response_

---

_Label `question` added by @vsashidh on 2026-01-15 22:36_

---

_Renamed from "editable dependency" to "editable dependency and name binding" by @vsashidh on 2026-01-15 22:38_

---

_Comment by @zanieb on 2026-01-15 22:39_

I think you're looking to understand https://typing.python.org/en/latest/spec/distributing.html#library-interface-public-and-private-symbols

---

_Comment by @vsashidh on 2026-01-15 22:53_

Thanks for your quick response.  I was hoping for a behavior as referenced here : https://docs.python.org/3/reference/import.html#submodules . Are you suggesting I use a list of module names in `__all__` to list out the submodules along with the relative import in `__init__.py` as an alternative approach?

EDIT: I ended up trying the above and synced the project referencing the library and it worked as I wanted to import.

---
