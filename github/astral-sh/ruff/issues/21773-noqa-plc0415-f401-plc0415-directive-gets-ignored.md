---
number: 21773
title: "noqa: PLC0415 F401 - PLC0415 directive gets ignored"
type: issue
state: closed
author: cleder
labels:
  - question
assignees: []
created_at: 2025-12-03T12:24:26Z
updated_at: 2025-12-03T13:45:33Z
url: https://github.com/astral-sh/ruff/issues/21773
synced_at: 2026-01-07T13:12:16-06:00
---

# noqa: PLC0415 F401 - PLC0415 directive gets ignored

---

_Issue opened by @cleder on 2025-12-03 12:24_

### Summary

I have an import inside a method of a class.

```python
class Foo:
    def bar(self):
        try:
            from foo import (
                baz,  # noqa: F401,PLC0415
            )
        except ImportError:
            foobar()


def foobar():
    from foo import (
        baz,  # noqa: F401,PLC0415
    )
```

```bash
> ruff check PLC0415.py --no-fix --ignore=D,ANN,N --select=ALL
PLC0415 `import` should be at the top-level of a file
 --> PLC0415.py:4:13
  |
2 |       def bar(self):
3 |           try:
4 | /             from foo import (
5 | |                 baz,  # noqa: F401,PLC0415
6 | |             )
  | |_____________^
7 |           except ImportError:
8 |               foobar()
  |

RUF100 [*] Unused `noqa` directive (unused: `PLC0415`)
 --> PLC0415.py:5:23
  |
3 |         try:
4 |             from foo import (
5 |                 baz,  # noqa: F401,PLC0415
  |                       ^^^^^^^^^^^^^^^^^^^^
6 |             )
7 |         except ImportError:
  |
help: Remove unused `noqa` directive

PLC0415 `import` should be at the top-level of a file
  --> PLC0415.py:12:5
   |
11 |   def foobar():
12 | /     from foo import (
13 | |         baz,  # noqa: F401,PLC0415
14 | |     )
   | |_____^
   |

RUF100 [*] Unused `noqa` directive (unused: `PLC0415`)
  --> PLC0415.py:13:15
   |
11 | def foobar():
12 |     from foo import (
13 |         baz,  # noqa: F401,PLC0415
   |               ^^^^^^^^^^^^^^^^^^^^
14 |     )
   |
help: Remove unused `noqa` directive
```

When the imports are all on the same line, it works



### Version

0.14.5

---

_Comment by @MichaReiser on 2025-12-03 12:59_

Hi @cleder 

Ruff, unlike e.g. Pyright, the ignore comment must be on the first line or last line. In your case, the `noqa` comment should be after the `(` or `)`:


```py
def test():
    try:
        from my_service.services import ( # noqa: PLC0415 F401
            schedule_sync_job_tasks,  
    )
    except ImportError:
        pass
```

```py
def test():
    try:
        from my_service.services import (
            schedule_sync_job_tasks,  
    )  # noqa: PLC0415 F401
    except ImportError:
        pass
```

---

_Label `question` added by @MichaReiser on 2025-12-03 12:59_

---

_Comment by @cleder on 2025-12-03 13:19_

confusing is that F401 MUST be on the same line:

```
class Foo:
    def bar(self):
        try:
            from foo import ( # noqa: PLC0415
                baz,  # noqa: F401
            )
        except ImportError:
            foobar()


def foobar():
    from foo import ( # noqa: PLC0415
        baz,  # noqa: F401
    )
```


---

_Comment by @MichaReiser on 2025-12-03 13:28_

Yeah, I can see how this might be a bit confusing. `F401` flags a specific import rather than the entire statement because there could be multiple once. `PLC0415` always flags the entire import statement, regardless of how many members you import, because the entire import needs to be moved (or allowed, as you do in this instance)

---

_Comment by @cleder on 2025-12-03 13:34_

The suggestion to put it after `)` does not work:

```
ruff check PLC0415.py --no-fix --ignore=D,ANN,N --select=ALL
PLC0415 `import` should be at the top-level of a file
 --> PLC0415.py:4:13
  |
2 |       def bar(self):
3 |           try:
4 | /             from foo import (
5 | |                 baz,  # noqa: F401
6 | |             )  # noqa: PLC0415
  | |_____________^
7 |           except ImportError:
8 |               foobar()
  |

RUF100 [*] Unused `noqa` directive (unused: `PLC0415`)
 --> PLC0415.py:6:16
  |
4 |             from foo import (
5 |                 baz,  # noqa: F401
6 |             )  # noqa: PLC0415
  |                ^^^^^^^^^^^^^^^
7 |         except ImportError:
8 |             foobar()
  |
help: Remove unused `noqa` directive

Found 2 errors.
[*] 1 fixable with the `--fix` option.
```

---

_Comment by @cleder on 2025-12-03 13:45_

adding the noqa via ruff works great btw:

`ruff check PLC0415.py --no-fix --ignore=D,ANN,N --select=ALL --add-noqa`

I guess I should use that more ;-)

Thanks for your help and ruff ❤️ 

---

_Closed by @cleder on 2025-12-03 13:45_

---
