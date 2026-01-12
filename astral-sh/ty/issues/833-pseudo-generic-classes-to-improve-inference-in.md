```yaml
number: 833
title: Pseudo-generic classes to improve inference in untyped code
type: issue
state: open
author: erictraut
labels:
  - feature
assignees: []
created_at: 2025-07-16T19:44:30Z
updated_at: 2025-07-16T20:07:30Z
url: https://github.com/astral-sh/ty/issues/833
synced_at: 2026-01-12T15:54:24Z
```

# Pseudo-generic classes to improve inference in untyped code

---

_@erictraut_

I was talking with @carljm, and I mentioned a feature in pyright that I've never really documented. He asked me to provide a brief description here for the benefit of the ty team. This feature is likely a low priority at this time, but it may be something to consider as ty's feature set matures.

This is a feature that I refer to as "pseudo-generic classes". When pyright encounters a non-generic class with an unannotated `__init__` method, it treats the class as though it's generic. Under the covers (and invisible to users), it creates a unique type variable for each `__init__` parameter. It then uses its normal type inference logic to infer the types of instance variables and method return types.

Here is an example:

Code sample in [pyright playground](https://pyright-play.net/?code=GYJw9gtgBANmDm8CWA7eUkQA5hAFygAkBDFAExgFMQAaKAOQHkAVAZQFFmAoLgYxmIBnQVACCAChLkqIAJQAuLlGVQylYFAD6m1EjzbxgyjGB1idAEZ0qAN2MBeJm04KlK94ICuWauNkA6bV19TXFbY1k3d2UjE0DgMDBNYih7KGIo6NjgeMTNC1SoCx53AAEscB98AE9MtQ0bYhhPSmTDY2BXaJUQSjxPEBQobNykjMzyyuo8Wvd6qEbm1ot2ky7uqF7%2BweGO0fyeYgBGQokAJgB2OgAia8jeuybNGZ9xY-9FluTZZQBiDBQeC4D0oTxelDeRw%2BTS%2BFh%2BUH%2BgjwIEOZ1O4ludAAzAAre6UR4wZ7VV7EM7QpbfP7DZHAgmgongt7kz7LeH-XiQLBUAAePH%2BzAAFkgRPBKChqMQ8JQRKQoNRwCAipReMRPEZ0lAsEZPGQwABaMUSkBIXhcDkCYQYEQoMAEYi8fpNGDVKBG6im-xcYhY%2BRiADaqDwdCRtBpIAAulwgA)

```python
from logging import Handler, NOTSET

class A(Handler):
    def __init__(self, a, b, level=NOTSET):
        super().__init__(level)
        self._foo_a = a
        self._foo_b = b

    @property
    def value_a(self):
        return self._foo_a

    @property
    def value_b(self):
        return self._foo_b

a1 = A(27, "")
reveal_type(a1.value_a)  # int
reveal_type(a1.value_b)  # str

a2 = A("", 3j)
reveal_type(a2.value_a)  # str
reveal_type(a2.value_b)  # complex

# This generates an error because a pseudo-generic
# class is not actually generic.
a3: A[int, str, str]

```

This feature should be able to use all of the normal type checker machinery (type inference, constraint solving, etc.), so all of that comes for "free". The tricky part is finding all of the places where type parameters and generic types are visible to users (diagnostic messages, revealed types, language server hover text, etc.) and make sure that these synthesized type parameters remain invisible to users.

---

_Label `feature` added by @carljm on 2025-07-16 20:07_

---
