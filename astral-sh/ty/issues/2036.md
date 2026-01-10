```yaml
number: 2036
title: "error[unresolved-attribute]: Module `sqlean` has no member `connect`"
type: issue
state: closed
author: kracekumar
labels: []
assignees: []
created_at: 2025-12-17T20:57:15Z
updated_at: 2025-12-17T22:07:28Z
url: https://github.com/astral-sh/ty/issues/2036
synced_at: 2026-01-10T01:53:59Z
```

# error[unresolved-attribute]: Module `sqlean` has no member `connect`

---

_Issue opened by @kracekumar on 2025-12-17 20:57_

### Summary

[Sqlean](https://github.com/nalgeon/sqlean.py) is sqlite3 compatible library. And ty is unable to find the attribute `connect` even though it's available. The sqlean is written in `C`. Not sure if this is the issue

Snippet

```
error[unresolved-attribute]: Module `sqlean` has no member `connect`
  --> tests/utils.py:24:12
   |
23 | def db_connection(dbname=":memory:"):
24 |     conn = sqlite3.connect(database=dbname, isolation_level=None)
   |            ^^^^^^^^^^^^^^^
25 |     return conn
   |
info: rule `unresolved-attribute` is enabled by default
```

Mypy: https://mypy-play.net/?mypy=latest&python=3.9&gist=34f8db88a3b9a8f609026eee9cd1ca7e
ty: https://play.ty.dev/7c1c9070-31bc-433f-8185-499fcb50d53c

### Version

ty 0.0.2 (42835578d 2025-12-16), python version: 3.9

---

_Comment by @carljm on 2025-12-17 21:22_

Thanks for the report!

The playground examples aren't very informative here, since neither mypy nor ty playground has any access to the `sqlean` module. The mypy example emits no diagnostics because it has `# mypy: ignore-errors` comment :) If that is removed, it emits an error that it can't find `sqlean` module.

When I try locally with `uv init` and then `uv add sqlean`, it doesn't seem that `sqlean.connect` exists at runtime, either:

```pycon
➜ uv run python
Python 3.13.2 (main, Mar 11 2025, 17:30:09) [Clang 20.1.0 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import sqlean
>>> sqlean.connect
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    sqlean.connect
AttributeError: module 'sqlean' has no attribute 'connect'
```

So it seems that ty is providing accurate type information here, and the reason your code type-checks under mypy is probably because of that `# mypy: ignore-errors` comment.

---

_Closed by @carljm on 2025-12-17 21:22_

---

_Comment by @kracekumar on 2025-12-17 21:47_

Thanks for the answer. I am replying since I have some extra information to share.

>When I try locally with uv init and then uv add sqlean, it doesn't seem that sqlean.connect exists at runtime, either:

It's weird. It seems to exist for me because there needs to be `sqlean and sqlean-py`and both needs to be installed.
Sample code

```
$cat a.py
import sqlean
print(sqlean.connect)
```

pyproject 

```
cat pyproject.toml
[project]
name = "sqlean-test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.9"
dependencies = [
    "mypy>=1.19.1",
    "sqlean>=0.0.3",
    "sqlean-py>=3.50.4.5",
    "ty>=0.0.2",
]
```

runtime does seem to consist of the function

```python
uv run a.py
<built-in function connect>
```

>The mypy example emits no diagnostics because it has # mypy: ignore-errors comment :) If that is removed, it emits an error that it can't find sqlean module.

ah, you're right. My apologies. Mypy do throw an error. I over-looked the ignore line at the top of the file.

```
$uv run mypy a.py
a.py:1: error: Skipping analyzing "sqlean": module is installed, but missing library stubs or py.typed marker  [import-untyped]
a.py:1: note: See https://mypy.readthedocs.io/en/stable/running_mypy.html#missing-imports
Found 1 error in 1 file (checked 1 source file)
```

ty

```
uv run ty check a.py
error[unresolved-attribute]: Module `sqlean` has no member `connect`
 --> a.py:2:7
  |
1 | import sqlean
2 | print(sqlean.connect)
  |       ^^^^^^^^^^^^^^
  |
info: rule `unresolved-attribute` is enabled by default
```

I will add ignore for sqlean. 

Thanks a lot for quick response!


---

_Comment by @carljm on 2025-12-17 22:07_

Thanks for the additional info!

If I install `sqlean-py` module, then I do indeed see `sqlean.connect` available at runtime. However, it seems to be made available at runtime as an import side effect, in a way that is not visible to type checkers. (I don't think mypy or any other type checker can see it either, it's just that mypy ignores the `sqlean` module altogether and treats it as unknown, whereas we find it and analyze its code.) So the solution here is either to ignore, or to add some type stubs for `sqlean` module.

---
