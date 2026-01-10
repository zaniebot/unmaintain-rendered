---
number: 4604
title: N805 ignores classmethod-decorators setting
type: issue
state: closed
author: ThiefMaster
labels: []
assignees: []
created_at: 2023-05-23T14:08:45Z
updated_at: 2023-10-23T08:15:44Z
url: https://github.com/astral-sh/ruff/issues/4604
synced_at: 2026-01-10T01:22:43Z
---

# N805 ignores classmethod-decorators setting

---

_Issue opened by @ThiefMaster on 2023-05-23 14:08_

### ruff.toml

```toml
select = ['N805']

[pep8-naming]
classmethod-decorators = [
    'classmethod',
    'declared_attr',
    'strict_classproperty',
    'expression',
    'comparator',
]
```

### ruff_sample.py

```python
from sqlalchemy.ext.hybrid import hybrid_property
from indico.util.decorators import strict_classproperty


class SQLAlchemyModel:
    @hybrid_property
    def foo(self):
        return not self._foo

    @foo.expression
    def foo(cls):
        return ~cls._foo

    @strict_classproperty
    def bar(cls):
        return cls._bar
```

### Output

```
$ ruff check ruff_sample.py
ruff_sample.py:11:13: N805 First argument of a method should be named `self`
ruff_sample.py:15:13: N805 First argument of a method should be named `self`
Found 2 errors.
```

### Versions
```
flake8-to-ruff==0.0.233
ruff==0.0.269
```

---

_Comment by @charliermarsh on 2023-05-23 14:30_

Ah, you need to provide fully-qualified paths, e.g., `classmethod-decorators = ["sqlalchemy.ext.hybrid.hybrid_property"]`. We match on the binding, not the symbol name, which ensures that (e.g.) you can import symbols under aliases and everything works as expected.

---

_Closed by @charliermarsh on 2023-05-23 14:30_

---

_Comment by @ThiefMaster on 2023-05-23 14:45_

This works fine for my `strict_classproperty` decorator but I don't see how this would work for the hybrids. The method decorated with `hybrid_property` itself does not become a classmethod. The `foo.expression` one does. And that's not something I can specify as a fully-qualified name...

---

_Comment by @ThiefMaster on 2023-10-23 08:15_

@charliermarsh ping :) I think this issue was closed too soon.

I just tested again using `ruff==0.1.1` and it still seems to be impossible to handle those classproperties...

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

Basically I need ruff to treat either `@*.expression` as a classmethod decorator, or alternatively have a way of telling it that `sqlalchemy.ext.hybrid.hybrid_property` returns a normal property, but the method decorated with that property has a `.expression` attribute that is a classmethod decorator...

Considering that this is a very common SQLAlchemy feature, and larger projects may have plenty of those, it's still a blocker for using ruff :/

---

_Referenced in [astral-sh/ruff#8397](../../astral-sh/ruff/issues/8397.md) on 2023-10-31 22:56_

---
