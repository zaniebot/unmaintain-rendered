```yaml
number: 19342
title: Emit better syntax error for unicode prefix
type: issue
state: open
author: dylwil3
labels:
  - parser
  - python314
assignees: []
created_at: 2025-07-14T22:00:45Z
updated_at: 2025-07-20T22:06:03Z
url: https://github.com/astral-sh/ruff/issues/19342
synced_at: 2026-01-12T15:54:56Z
```

# Emit better syntax error for unicode prefix

---

_@dylwil3_

Currently we get:

```console
❯ echo 'uf"a"' | ruff check --output-format concise -
-:1:3: SyntaxError: Simple statements must be separated by newlines or semicolons
Found 1 error.
```

but ideally we would get the more informative (as of Python 3.14):

```pycon
>>> uf"a"
  File "<python-input-0>", line 1
    uf"a"
    ^^
SyntaxError: 'u' and 'f' prefixes are incompatible
```

and similarly for t-strings.

---

_Label `parser` added by @dylwil3 on 2025-07-14 22:00_

---

_Comment by @MeGaGiGaGon on 2025-07-15 00:34_

This also applies to byte strings:
```
>echo 'bf""' | uvx ruff check --output-format concise -
-:1:3: SyntaxError: Simple statements must be separated by newlines or semicolons
Found 1 error.
```
And all other incompatible string prefixes

---

_Comment by @MeGaGiGaGon on 2025-07-15 00:38_

Hm, I wonder when this was added, since I don't get extra info on 3.14
```
>uvx python@3.14 -c 'uf"a"'
  File "<string>", line 1
    uf"a"
      ^^^
SyntaxError: invalid syntax
>uvx python@3.14 --version
Python 3.14.0a6
```

---

_Comment by @dylwil3 on 2025-07-15 03:27_

I think here: https://github.com/python/cpython/issues/133197

uv now ships the latest beta for Python 3.14 so you should be able to update to that one!

---

_Label `python314` added by @dylwil3 on 2025-07-20 22:06_

---
