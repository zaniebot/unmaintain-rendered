---
number: 12897
title: "[Infinite loop] Package-relative import issue with module `__all__` when using F401 and F822 with the preview option"
type: issue
state: closed
author: AA-Turner
labels:
  - bug
assignees: []
created_at: 2024-08-14T21:24:50Z
updated_at: 2025-01-16T16:15:19Z
url: https://github.com/astral-sh/ruff/issues/12897
synced_at: 2026-01-07T13:12:15-06:00
---

# [Infinite loop] Package-relative import issue with module `__all__` when using F401 and F822 with the preview option

---

_Issue opened by @AA-Turner on 2024-08-14 21:24_

This took a while to minimise, hopefully it is indicative of a real issue!

There's some setup involved, so you can use the following ``reproducer.py`` in an empty directory.

```python
# reproducer.py
from pathlib import Path


def write(path, text):
    path.write_text(text, encoding="utf-8")


BUG = """\
__all__ = ('Spam',)


class Spam:
    def __init__(self) -> None:
        from bug.lobster_thermidor import Ham
"""

MOD_INIT = Path('bug/__init__.py')
MOD_LOBSTER = Path('bug/lobster_thermidor.py')
MOD_INIT.parent.mkdir(exist_ok=True)
BUG_FILE = Path('bug.py')
write(MOD_INIT, BUG)
write(BUG_FILE, BUG)
write(MOD_LOBSTER, 'Ham = 1\n')
```

This is the same as the following structure:

```
bug.py
bug/
  -> __init__.py
  -> lobster_thermidor.py
```

In both ``bug.py`` and ``bug/__init__.py`` we have:

```python
__all__ = ('Spam',)


class Spam:
    def __init__(self) -> None:
        from bug.lobster_thermidor import Ham
```

In ``lobster_thermidor.py`` we have:

```python
Ham = 1
```

The issue is then straightforwards to reproduce:

```ps1con
PS> ruff -V
ruff 0.5.7
PS> ruff check --isolated --select F401,F822 --preview --fix bug.py
Found 1 error (1 fixed, 0 remaining).
PS> ruff check --isolated --select F401,F822 --preview --fix bug/

error: Failed to converge after 100 iterations.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `bug\__init__.py`, the rule codes F401, F822, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

bug\__init__.py:1:20: F822 Undefined name `Ham` in `__all__`
  |
1 | __all__ = ('Spam', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham
', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ha
m', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'H
am', 'Ham', 'Ham', 'Ham', 'Ham',)
  |                    ^^^^^ F822
  |
[ ... 1023(!!!) lines of output elided ... ]

Found 201 errors (100 fixed, 101 remaining).
[*] 1 fixable with the `--fix` option.
```

This only reproduces with the ``--preview`` option. Clearly there is a bad recursion or loop bug somewhere. If it is an topic for discussion though, I don't think an auto-fix should add an import to ``__all__`` -- the correct solution (as observed in ``bug.py``) is to remove the import.

Thanks, and please let me know if any further information is needed,
Adam

---

_Renamed from "[Infinite loop]" to "[Infinite loop] Package-relative import issue with module ``_all__`` when using F401 and F822 with the preview option" by @AA-Turner on 2024-08-14 21:26_

---

_Comment by @AA-Turner on 2024-08-14 21:26_

Sorry, I forgot to set a title.

A

---

_Renamed from "[Infinite loop] Package-relative import issue with module ``_all__`` when using F401 and F822 with the preview option" to "[Infinite loop] Package-relative import issue with module `__all__` when using F401 and F822 with the preview option" by @AlexWaygood on 2024-08-14 22:05_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-08-15 07:46_

---

_Label `bug` added by @MichaReiser on 2024-08-19 13:28_

---

_Referenced in [astral-sh/ruff#15411](../../astral-sh/ruff/pulls/15411.md) on 2025-01-11 08:54_

---

_Comment by @ntBre on 2025-01-15 19:02_

I can reproduce the loop without F822 on ruff 0.8.2 and on the current main branch:

```shell
$ mkdir -p bug
$ python reproducer.py
$ ~/astral/ruff/target/debug/ruff check --no-cache --isolated --select F401 --preview --fix bug/

# output
debug error: Failed to converge after 100 iterations in `bug/__init__.py` with rule codes F401:---
__all__ = ('Spam', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham', 'Ham',)


class Spam:
    def __init__(self) -> None:
        from bug.lobster_thermidor import Ham

---
bug/__init__.py:6:43: F401 `bug.lobster_thermidor.Ham` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
  |
4 | class Spam:
5 |     def __init__(self) -> None:
6 |         from bug.lobster_thermidor import Ham
  |                                           ^^^ F401
  |
  = help: Add unused import `Ham` to __all__

Found 101 errors (100 fixed, 1 remaining).
``` 

F822 makes the message much, much longer, but I think the actual problem is caused by F401.

---

_Assigned to @ntBre by @ntBre on 2025-01-15 19:02_

---

_Unassigned @AlexWaygood by @AlexWaygood on 2025-01-15 19:07_

---

_Referenced in [astral-sh/ruff#15517](../../astral-sh/ruff/pulls/15517.md) on 2025-01-15 22:25_

---

_Closed by @ntBre on 2025-01-16 15:43_

---

_Comment by @AA-Turner on 2025-01-16 16:12_

Thank you @ntBre!

A

---

_Comment by @AlexWaygood on 2025-01-16 16:14_

Thanks @AA-Turner for reporting, and sorry it took us a little while to fix this one!

---

_Comment by @AA-Turner on 2025-01-16 16:15_

I have issues that have been open for over a decade ... five months is nothing!

A

---
