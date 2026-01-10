```yaml
number: 983
title: False positive for inconsistent-mro
type: issue
state: closed
author: LoicRiegel
labels: []
assignees: []
created_at: 2025-08-14T10:20:57Z
updated_at: 2025-08-14T16:10:05Z
url: https://github.com/astral-sh/ty/issues/983
synced_at: 2026-01-10T02:06:24Z
```

# False positive for inconsistent-mro

---

_Issue opened by @LoicRiegel on 2025-08-14 10:20_

### Summary

Hi,
I discovered a situation where an "inconsistent-mro" error is detected but that is a false positive: there is no TypeError at runtime.

I'm not a metaclass expert, so I'm not sure if it's a bug from ty or if something else is going on, but I still wanted to report that. Maybe it has to do with the C++ bindings inside shiboken?

**Steps to reproduce**

```sh
uv init
uv add PySide6
# copy content below into main.py
uvx ty --version
uvx ty check
uv run main.py
```

Content to paste "main.py":
```py
from abc import ABCMeta
from PySide6.QtCore import QObject


class Test(type(QObject), ABCMeta):
    pass
```

Output:
```
uvx ty check
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                                                    
error[inconsistent-mro]: Cannot create a consistent method resolution order (MRO) for class `Test` with bases list `[<class 'type'>, <class 'ABCMeta'>]`
 --> main.py:5:1
  |
5 | / class Test(type(QObject), ABCMeta):
6 | |     pass
  | |________^
  |
info: rule `inconsistent-mro` is enabled by default

Found 1 diagnostic
```

However "uv run main.py" runs just fine.

Expected (I guess):
ty does not raise any error, since the code does not throw a TypeError (as stated in the documentation page of this error).


### Version

ty 0.0.1-alpha.17 (1712284c8 2025-08-06)

---

_Comment by @LoicRiegel on 2025-08-14 10:23_

By the way I wanted to find a single uv command to reproduce the problem without having to create the project and everything but I couldn't find it.
I tried things like ``uvx --no-project --with PySide6 ty check main.py``...
If it's possible let me know ^^

---

_Comment by @carljm on 2025-08-14 16:07_

Thanks for the report!

I think `inconsistent-mro` is a bit of a red herring here. The root of the problem is that ty understands the type of `type(QObject)` to be just the class object `builtins.type`. Trying to multiply inherit `type` and `ABCMeta` does result in an inconsistent-mro error at runtime:

```
>>> from abc import ABCMeta
>>> class C(type, ABCMeta): ...
...
Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    class C(type, ABCMeta): ...
TypeError: Cannot create a consistent method resolution order (MRO) for bases type, ABCMeta
```

So given ty's understanding of the types of the two bases, I think we are issuing the `inconsistent-mro` error correctly. The question is why we understand the type of `type(QObject)` to be `<class 'type'>` instead of `Shiboken.ObjectType` as it is at runtime.

And I think the reason for this is inaccurate stubs. The `QtCore.pyi` which I find installed with PySide6 just shows `QObject` as a regular class inheriting from `Shiboken.Object`, and the [shiboken stub](https://github.com/pyside/pyside-setup/blob/dev/sources/shiboken6/shibokenmodule/Shiboken.pyi#L16) shows `Object` as a perfectly ordinary class; no metaclass or anything. It doesn't even define anything named `ObjectType`.

So given the type information ty is working from, I think inferring the type of `type(QObject)` as `<class 'type'>` is correct, and therefore the `inconsistent-mro` error is also correct.

FWIW, I tested this same snippet with mypy and pyright. Mypy issues an even more basic "Unsupported dynamic base class 'type'" error, and pyright issues the same inconsistent MRO error that we do.

So I don't think there is any bug in ty here; if you want this code to type-check, it will require improvements to the PySide6 and/or Shiboken stubs, so that they even recognize the existence of `Shiboken.ObjectType` (the thing you are trying to inherit from) in the first place.

> wanted to find a single uv command to reproduce the problem without having to create the project and everything

Hmm, I'm not aware of how to do this either. I suspect the issue with using `--with` is that it creates the venv in a place where ty can't find it? Although I'm not sure why it doesn't set the `VIRTUAL_ENV` environment variable.



---

_Closed by @carljm on 2025-08-14 16:07_

---
