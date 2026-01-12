```yaml
number: 680
title: E999 syntax error for match statement
type: issue
state: closed
author: thosil
labels: []
assignees: []
created_at: 2022-11-11T10:32:14Z
updated_at: 2023-02-23T00:10:07Z
url: https://github.com/astral-sh/ruff/issues/680
synced_at: 2026-01-12T15:54:40Z
```

# E999 syntax error for match statement

---

_@thosil_

ruff throw a syntax error for code like this:
```python3
import sys

value: str = sys.argv[1]

match value:
    case "ruff":
        print("Rough?")
    case "awesome":
        print("Of course is it!")
    case _:
        print("what?")

```

```
$ ruff ruff_and_match.py
Found 1 error(s).
ruff_and_match.py:5:8: E999 SyntaxError: invalid syntax. Got unexpected token 'value'
```

### versions
- Python 3.10.6
- ruff 0.0.108

### config:
```toml
[tool.ruff]
line-length = 100
select=["E", "F", "W", "U", "N", "C", "B", "A", "T", "Q", "RUF", "M", "ANN"]
ignore=["F403", "F405", "B008", "A003"]

```

(and yes ruff is awesome ;-))

---

_Comment by @squiddy on 2022-11-11 10:59_

Yeah, ruff is using RustPython which doesn't support match yet. 

Also see https://github.com/charliermarsh/ruff/issues/282 and https://github.com/charliermarsh/ruff/issues/54

There is a note in the README, but it's a bit hidden.

> Ruff does not yet support a few Python 3.9 and 3.10 language features, including structural pattern matching and parenthesized context managers.

---

_Comment by @thosil on 2022-11-11 11:57_

ok, sorry for the noise :grimacing: 

---

_Closed by @thosil on 2022-11-11 11:57_

---

_Comment by @squiddy on 2022-11-11 12:08_

No worries, just pointing it out. :) I think Charlie is working on alternatives to RustPython that could fix that.

---

_Comment by @charliermarsh on 2022-11-11 15:12_

Yeah, sadly those aren't supported yet. Another possibility is rewriting the RustPython parser to support that syntax! (It has to be rewritten anyway, CPython itself moved to a new parser to support those expressions which were "impossible" in the old parser.)


---

_Comment by @charliermarsh on 2022-12-15 13:18_

(For clarity: this is still open, but a dupe of #282.)

---

_Comment by @charliermarsh on 2023-02-22 00:05_

Fixed as of v0.0.250 which included `match` statement support.

---

_Comment by @eddyg on 2023-02-23 00:08_

Holy cow! 🎉 That's fantastic news, Charlie! Can't wait to replace all my `elif` chains with `match` statements now. 😄 

---

_Comment by @charliermarsh on 2023-02-23 00:10_

🎸 🎸 🎸 

---
