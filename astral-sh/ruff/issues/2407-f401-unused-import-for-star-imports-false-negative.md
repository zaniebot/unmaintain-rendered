```yaml
number: 2407
title: "F401 unused-import for star-imports: false negative?"
type: issue
state: closed
author: spaceone
labels:
  - wontfix
assignees: []
created_at: 2023-01-31T18:39:10Z
updated_at: 2024-03-02T12:26:37Z
url: https://github.com/astral-sh/ruff/issues/2407
synced_at: 2026-01-12T15:54:42Z
```

# F401 unused-import for star-imports: false negative?

---

_@spaceone_

`flake8` and `ruff` differ for `*` imports:
```
$ flake8 - <<<"from foo import *"
stdin:1:1: F403 'from foo import *' used; unable to detect undefined names
stdin:1:1: F401 'foo.*' imported but unused
$ ruff - <<<"from foo import *"
-:1:1: F403 `from foo import *` used; unable to detect undefined names
Found 1 error.
```

I detected this as prior it had a `# noqa: F401` comment which was removed by `RUF100` when I compared any leftovers with `flake8`.

---

_Comment by @spaceone on 2023-01-31 18:44_

I filed an upstream bug: https://github.com/PyCQA/pyflakes/issues/766
I would vote for fixing it in `pyflakes`.

---

_Comment by @spaceone on 2023-01-31 19:00_

So they think that if `F405` doesn't occur they mark the star import as `F401`.

`F405` can be triggered by:
```sh
$ python3 -m pyflakes <<< $'from foo import *; __all__ = ("a",)'
<stdin>:1:1: 'from foo import *' used; unable to detect undefined names
<stdin>:1:20: 'a' may be undefined, or defined from star imports: foo
$ python3 -m pyflakes <<< $'from foo import *; a'
<stdin>:1:1: 'from foo import *' used; unable to detect undefined names
<stdin>:1:20: 'a' may be undefined, or defined from star imports: foo
```

---

_Comment by @charliermarsh on 2023-01-31 19:12_

I think I'm okay with Ruff's behavior here but open to being convinced otherwise. What do you think?

---

_Label `question` added by @charliermarsh on 2023-01-31 19:12_

---

_Comment by @spaceone on 2023-01-31 19:15_

I am unsure. I like ruffs behavior more. Will think what I can do for the time I need to support both flake8 and ruff (aka: until #2402 is fixed).

---

_Comment by @ngnpope on 2023-01-31 20:10_

For a file containing just the following (which is often seen in `__init__.py`):
```python
from math import *
```
```console
❯ flake8 bug.py
bug.py:1:1: F403 'from math import *' used; unable to detect undefined names
bug.py:1:1: F401 'math.*' imported but unused
❯ ruff bug.py 
bug.py:1:1: F403 `from math import *` used; unable to detect undefined names
Found 1 error.
```
Using something from the `math` module:
```python
from math import *
pi
```
```console
❯ flake8 bug.py
bug.py:1:1: F403 'from math import *' used; unable to detect undefined names
bug.py:2:1: F405 'pi' may be undefined, or defined from star imports: math
❯ ruff bug.py 
bug.py:1:1: F403 `from math import *` used; unable to detect undefined names
bug.py:2:1: F405 `pi` may be undefined, or defined from star imports: `math`
Found 2 errors.
```
Using something that is not in the `math` module:
```python
from math import *
invalid_name
```
```console
❯ flake8 bug.py
bug.py:1:1: F403 'from math import *' used; unable to detect undefined names
bug.py:2:1: F405 'invalid_name' may be undefined, or defined from star imports: math
❯ ruff bug.py 
bug.py:1:1: F403 `from math import *` used; unable to detect undefined names
bug.py:2:1: F405 `invalid_name` may be undefined, or defined from star imports: `math`
Found 2 errors.
```

It has always bugged me getting `F401` and `F403` when the latter feels like it gives the expectation of the former, so I do quite like the way `ruff` does this... But I wouldn't be upset if it was implemented like the original.

---

_Comment by @charliermarsh on 2023-02-02 03:23_

Going to close for now because I'm comfortable with our current behavior.

---

_Closed by @charliermarsh on 2023-02-02 03:23_

---

_Label `question` removed by @charliermarsh on 2023-02-02 03:23_

---

_Label `wontfix` added by @charliermarsh on 2023-02-02 03:23_

---

_Comment by @enriquebos on 2023-12-07 20:02_

I think if __all__ is defined it should recognize the star import, only throw if __all__ is also undefined.

---

_Comment by @niniack on 2024-03-02 12:25_

> I think if **all** is defined it should recognize the star import, only throw if **all** is also undefined.

I think I agree with this, but it would be good to hear if someone thinks there is a reason why this is bad practice for `__init__.py` files. Anyways, it was really bothering me so I used the following in my pyproject.toml, adapted from [here](https://stackoverflow.com/questions/77680073/ignore-specific-rules-in-specific-directory-with-ruff)

```
[tool.ruff.lint.per-file-ignores]
"__init__.py" = ["F401", "F403"]
```


---
