```yaml
number: 15172
title: "Pyflakes rules do not detect unused shadowed import when using `from __future__ import annotations`"
type: issue
state: closed
author: injust
labels: []
assignees: []
created_at: 2024-12-28T21:48:30Z
updated_at: 2024-12-30T04:57:15Z
url: https://github.com/astral-sh/ruff/issues/15172
synced_at: 2026-01-12T15:54:54Z
```

# Pyflakes rules do not detect unused shadowed import when using `from __future__ import annotations`

---

_@injust_

```python
from __future__ import annotations

from enum import Enum

from packaging.version import Version


class Foo:
    class Version(Enum):
        pass

    version: Version | None = None
```

Inside `Foo`, the unused `from packaging.version import Version` import is shadowed by the `Version` enum. But Ruff doesn't detect the unused import. If I remove `from __future__ import annotations`, F401 and F811 are correctly reported.

Pyflakes output:

```
test.py:5:1: 'packaging.version.Version' imported but unused
test.py:9:5: redefinition of unused 'Version' from line 5
```

---

_Comment by @Daverball on 2024-12-29 08:26_

This is a bit of a tricky case. Technically this is ill-defined as far as type checkers are concerned, since with `from __future__ import annotations` analysis can happen out of order, so the semantics need to remain the same, no matter in which order you analyze the statements (as long as you defer lookups when you can't find something), so when the lookup for `Version` happens it might have seen `Version` in the global scope, but not the one the local scope.

Ruff performs a global name lookup in deferred annotations in order to resolve that ambiguity, which is why the import doesn't get detected as unused. The safe way to write this would be:

```python
from __future__ import annotations

from enum import Enum

from packaging.version import Version


class Foo:
    class Version(Enum):
        pass

    version: Foo.Version | None = None
```

Which does trigger F401 and F811

---

_Comment by @Daverball on 2024-12-29 09:04_

Also this matches the behavior of `typing.get_type_hints`, which will perform the lookup in the global scope before looking it up in the local scope:

```pycon
>>> from __future__ import annotations
>>> from collections.abc import Iterable                                                   
>>> class Foo:
...     class Iterable:
...             pass
...     iterable: Iterable | None = None                                                   
... 
>>> from typing import get_type_hints                                                      
>>> get_type_hints(Foo)
{'iterable': collections.abc.Iterable | None}
```

---

_Comment by @injust on 2024-12-30 04:57_

I see, thanks for explaining.

---

_Closed by @injust on 2024-12-30 04:57_

---
