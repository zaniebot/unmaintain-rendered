```yaml
number: 68
title: Results that do not meet expectations
type: issue
state: closed
author: kolapapa
labels: []
assignees: []
created_at: 2022-09-01T03:29:49Z
updated_at: 2022-09-01T17:04:55Z
url: https://github.com/astral-sh/ruff/issues/68
synced_at: 2026-01-12T15:54:40Z
```

# Results that do not meet expectations

---

_@kolapapa_

`main.py`
```python
from flask import Flask

def make_app():
    app = Flask(__name__)
    return app

app = make_app()
```

Results:
```shell
$ ruff main.py
Found 1 error(s).

main.py:7:7: F821 Undefined name `make_app`
```

```shell
$ flake8 main.py
main.py:3:1: E302 expected 2 blank lines, found 1
main.py:7:1: E305 expected 2 blank lines after class or function definition, found 1
```

---

_Comment by @charliermarsh on 2022-09-01 13:13_

I believe the F821 false-positive was fixed in #57 (which went out as part of 0.0.21).

E302 and E305 aren't implemented yet.


---

_Comment by @charliermarsh on 2022-09-01 17:04_

Going to close this for now because I'm not yet tracking individual issues for new rules. Thank you for posting :)

---

_Closed by @charliermarsh on 2022-09-01 17:04_

---
