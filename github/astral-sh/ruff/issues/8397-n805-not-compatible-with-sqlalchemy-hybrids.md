---
number: 8397
title: N805 not compatible with sqlalchemy hybrids
type: issue
state: closed
author: ThiefMaster
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2023-10-31T22:56:16Z
updated_at: 2023-11-10T02:04:27Z
url: https://github.com/astral-sh/ruff/issues/8397
synced_at: 2026-01-07T13:12:15-06:00
---

# N805 not compatible with sqlalchemy hybrids

---

_Issue opened by @ThiefMaster on 2023-10-31 22:56_

Apologies for opening a new issue for what's basically part of #4604, but considering that this is one closed even though half of it has not been addressed at all, and I'm not getting any replies to my comment there, I think opening a new one makes sense - this is clearly a missing feature / bug, and it makes this particular check from ruff pretty much unusable for any codebase using SQLAlchemy's hybrid properties.

Basically I need ruff to treat either `@*.expression` as a classmethod decorator, or alternatively have a way of telling it that `sqlalchemy.ext.hybrid.hybrid_property` returns a normal property, but the method decorated with that property has a `.expression` attribute that is a classmethod decorator...


---

### ruff.toml

```toml
select = ['N805']

[pep8-naming]
classmethod-decorators = [
    'classmethod',
    'declared_attr',
    'expression',
    'comparator',
]
```

### ruff_sample.py

```python
from sqlalchemy.ext.hybrid import hybrid_property


class SQLAlchemyModel:
    @hybrid_property
    def foo(self):
        return not self._foo

    @foo.expression
    def foo(cls):
        return ~cls._foo
```

### Output

```
$ ruff check ruff_sample.py
ruff_sample.py:10:13: N805 First argument of a method should be named `self`
Found 1 error.
```

### Version
```
ruff==0.1.3
```

---

_Comment by @zanieb on 2023-11-01 00:30_

Perhaps we can allow wildcards in `classmethod-decorators`; would that address your use case?

Sorry your last issue didn't get more replies, we're a small team :)

---

_Comment by @ThiefMaster on 2023-11-01 00:34_

i think one of these solutions would be the easiest and indeed solve my issue:

- wildcard support as you proposed, applied to the whole decorator function name
- doing a suffix match when the entry in classmethod-decorators starts with a dot

---

_Label `configuration` added by @zanieb on 2023-11-01 00:45_

---

_Label `needs-decision` added by @zanieb on 2023-11-01 00:45_

---

_Referenced in [astral-sh/ruff#8592](../../astral-sh/ruff/pulls/8592.md) on 2023-11-09 23:25_

---

_Closed by @charliermarsh on 2023-11-10 02:04_

---
