```yaml
number: 8247
title: False positive for SIM300 (yoda condition)?
type: issue
state: closed
author: estyrke
labels: []
assignees: []
created_at: 2023-10-26T09:01:01Z
updated_at: 2023-10-31T07:04:51Z
url: https://github.com/astral-sh/ruff/issues/8247
synced_at: 2026-01-12T15:54:47Z
```

# False positive for SIM300 (yoda condition)?

---

_@estyrke_

`sim300.py`:
```python
class Foo:
    NAME = "Foo"

class Bar:
    NAME = "Bar"

def check(name: str) -> None:
    for item in [Foo(), Bar()]:
        if item.NAME == name:
            print(item)
```

```shell
$ ruff --version
ruff 0.0.291
$ ruff check --isolated --select SIM300 sim300.py
sim300.py:9:12: SIM300 [*] Yoda conditions are discouraged, use `name == item.NAME` instead
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

Applying the suggested fix results in *more* Yoda-ish order in my opinion.


---

_Comment by @charliermarsh on 2023-10-30 23:37_

I think this is working as intended -- `name` is the input to the function, so `name == item.NAME` would be the non-Yoda variant, though it's obviously a bit subjective.

---

_Closed by @charliermarsh on 2023-10-30 23:37_

---

_Comment by @estyrke on 2023-10-31 07:04_

I would argue that in the loop, `item` is more variable than `name`. The rule should use the outermost "object" (item) and not the inner one (NAME) to determine const-ness. I could accept that it is subjective, but then ruff shouldn't complain for any of the orders.

---
