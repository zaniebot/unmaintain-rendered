```yaml
number: 1969
title: Go to Definition not work for module
type: issue
state: closed
author: Liyixin95
labels:
  - bug
  - server
assignees: []
created_at: 2025-12-17T01:48:21Z
updated_at: 2025-12-18T07:28:08Z
url: https://github.com/astral-sh/ty/issues/1969
synced_at: 2026-01-12T15:54:26Z
```

# Go to Definition not work for module

---

_@Liyixin95_

### Summary

dir structure
```
❯ lsd --tree                                                                                                                                                                                                                     (base)
 .
├──  __init__.py
├──  module1
│   └──  __init__.py
├──  module2.py
└──  test.py
```

code in test.py
```python
from test import module1, module2
```

call `go to definition` on `module1` and `module2` can not jump to the module path.

### Version

0.0.2

---

_Comment by @zanieb on 2025-12-17 04:06_

Why would `module1` and `module2` be importable from `test` in your example?

e.g., if you look at this playground example https://play.ty.dev/59aaa507-8a05-4b8a-a5eb-6267ee59828e

`module1` and `module2` are importable,  but not via `from test`, which I'd expect since they aren't submodules of `test`?

---

_Comment by @Liyixin95 on 2025-12-17 05:12_

> Why would `module1` and `module2` be importable from `test` in your example?
> 
> e.g., if you look at this playground example https://play.ty.dev/59aaa507-8a05-4b8a-a5eb-6267ee59828e
> 
> `module1` and `module2` are importable, but not via `from test`, which I'd expect since they aren't submodules of `test`?

sorry for missing information.  the root module is test, the test.py is a name chosen at random. change it to test1.py get the same result. 

```
 .
└── 󰙨 test
    ├──  __init__.py
    ├──  module1
    │   └──  __init__.py
    ├──  module2.py
    └──  test.py
```
And I have tried pyrefly in this case. Pyrefly can jump to`test/module1/__init__.py` or `test/module2.py` by call go to definition.

---

_Label `server` added by @AlexWaygood on 2025-12-17 07:46_

---

_Comment by @MichaReiser on 2025-12-17 08:26_

Did you open the `test` folder in the editor or is it any other folder? Do you have any `pyproject.toml` or `ty.toml` files in that or any of its ancestor directories?

Go-to definition is working for me if I open `test`'s parent folder in VS Code. The imports don't resolve if I open `test` directly (because ty then thinks `test` is the root of your project`)

---

_Label `needs-info` added by @MichaReiser on 2025-12-17 08:26_

---

_Comment by @Liyixin95 on 2025-12-17 09:59_

> Did you open the `test` folder in the editor or is it any other folder? Do you have any `pyproject.toml` or `ty.toml` files in that or any of its ancestor directories?
> 
> Go-to definition is working for me if I open `test`'s parent folder in VS Code. The imports don't resolve if I open `test` directly (because ty then thinks `test` is the root of your project`)

<!-- Failed to upload "test1.py - ty-test [SSH_ remote] - Visual Studio Code 2025-12-17 17-53-26.mp4" -->

https://github.com/user-attachments/assets/9254c268-6bd4-4182-9989-e1fccaed613d

---

_Comment by @MichaReiser on 2025-12-17 10:12_

Can you try opening the *parent* folder of the `tests` directory?

---

_Comment by @Liyixin95 on 2025-12-17 10:22_

> Can you try opening the _parent_ folder of the `tests` directory?

The folder i opened is `ty-test`, which is already the parent of test.

![Image](https://github.com/user-attachments/assets/bb93447e-94da-40f7-856f-40e63cb3a24a)

---

_Label `needs-info` removed by @MichaReiser on 2025-12-17 10:23_

---

_Label `bug` added by @MichaReiser on 2025-12-17 10:23_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-17 10:23_

---

_Comment by @MichaReiser on 2025-12-17 10:24_

Test:

```rust
        let test = CursorTest::builder()
            .source("lib/__init__.py", r#""#)
            .source("lib/module.py", r#""#)
            .source("main.py", r#"from lib import module<CURSOR>"#)
            .build();
```

Returns no goto def targets

---

_Comment by @MichaReiser on 2025-12-17 10:38_

I think what we need is a fallback in `resolve_from_import_definitions` to resolve the full module name and return its definition. @Gankra is this something you could look into? I think you'll be much faster at fixing this than I

---

_Assigned to @Gankra by @Gankra on 2025-12-17 14:22_

---

_Comment by @Gankra on 2025-12-17 14:25_

Positively baffled I'm sure we implemented this, but yep it doesn't work right now.

---

_Closed by @MichaReiser on 2025-12-18 07:28_

---
