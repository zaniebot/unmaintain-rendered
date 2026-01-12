```yaml
number: 2214
title: TCH rules require from __future__ import annotations
type: issue
state: closed
author: twoertwein
labels:
  - bug
assignees: []
created_at: 2023-01-26T18:33:56Z
updated_at: 2024-06-04T14:17:20Z
url: https://github.com/astral-sh/ruff/issues/2214
synced_at: 2026-01-12T15:54:42Z
```

# TCH rules require from __future__ import annotations

---

_@twoertwein_

The following does not work with 3.11.1 at runtime:


test.py
```py
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from test_b import B

def test(x: B) -> B:
    return x
```

test_b.py
```py
class B:
    ...
```

```sh
$ python test.py 
Traceback (most recent call last):
  File "test.py", line 9, in <module>
    def test(x: B) -> B:
                ^
NameError: name 'B' is not defined
```

but it works when `test.py `contains `from __future__ import annotations`.


It might be good to either skip the TCH rules when `from __future__ import annotations` is not there or mention that this import needs to be added.



---

_Comment by @charliermarsh on 2023-01-26 18:59_

Hmm yeah. [`flake8-type-checking`](https://github.com/snok/flake8-type-checking#choosing-how-to-handle-forward-references) has some things to say about that and additional rules. @sondrelg - I promise I'll stop pinging you, but anything else you'd say about the "right" way to handle this, based on your experience?

---

_Comment by @sondrelg on 2023-01-26 20:23_

Yeah this is correct for all Python version above 3.7 I believe. Ideally ruff should implement `TC100`, `TC101`, `TC200`, and `TC201` as these are meant to complement the `TC00[1,2,3]` rule by telling you how to handle forward references.

What happens we when put an import inside a `TYPE_CHECKING` block (or any condition that is evaluated as `False` at runtime) is that the code path is not evaluated, so the import effectively doesn't happen at runtime. Since code outside the condition cannot reference something that isn't defined, we either wrap the undefined values in quotes or add a `from __futures__ import annotations` since this enables [PEP563](https://peps.python.org/pep-0563/) behavior.

tl;dr: The `TC00[1,2,3]` need either  the `TC1*` or `TC2*` range errors enabled (not both) to guide users into setting up their code in a valid way. 

Sorry for not noticing this earlier. I should have warned you about this. And no worries about the pings - happy to help :+1: 

---

_Comment by @sondrelg on 2023-01-26 20:24_

This section explains the concepts adequately, I hope: https://github.com/snok/flake8-type-checking#choosing-how-to-handle-forward-references

---

_Comment by @RayJameson on 2023-01-30 06:45_

So for now there is no way to turn TC1/TC2 in ruff?

---

_Comment by @sondrelg on 2023-01-30 07:54_

The code just has to be written I think @RayJameson. There shouldn't be any real blockers in place.

---

_Comment by @charliermarsh on 2023-01-30 12:12_

Yup that's right -- we don't support those rules yet, only [TC001 - TC005](https://github.com/charliermarsh/ruff#flake8-type-checking-tch).

---

_Label `bug` added by @charliermarsh on 2023-02-28 22:44_

---

_Comment by @charliermarsh on 2023-02-28 22:44_

I think this specific issue was actually a bug that we fixed around runtime requirements for annotations in function scopes.

---

_Closed by @charliermarsh on 2023-02-28 22:44_

---

_Comment by @charliermarsh on 2023-02-28 22:45_

(That snippet no longer raises an error.)

---

_Comment by @Faithfinder on 2024-06-04 14:17_

Sorry for necroposting, but I've been studying this for the last couple of hours and don't get it. Is `from __future__ import annotation` needed with these rules or not? If it is, why doesn't ruff add it? 

---
