```yaml
number: 17812
title: "Type `<module 'hashlib'>` has no attribute `sha256`"
type: issue
state: closed
author: charliermarsh
labels:
  - ty
assignees: []
created_at: 2025-05-03T14:19:28Z
updated_at: 2025-05-07T15:22:02Z
url: https://github.com/astral-sh/ruff/issues/17812
synced_at: 2026-01-12T15:54:56Z
```

# Type `<module 'hashlib'>` has no attribute `sha256`

---

_@charliermarsh_

```python
import hashlib

m = hashlib.sha256()
m.update(b"Nobody inspects")
m.update(b" the spammish repetition")
m.digest()
```

https://types.ruff.rs/ead42c11-58c3-465a-a842-40f3df8191f8


---

_Label `red-knot` added by @charliermarsh on 2025-05-03 14:19_

---

_Closed by @AlexWaygood on 2025-05-03 14:25_

---

_Comment by @AlexWaygood on 2025-05-03 14:32_

Explanation of why this is a duplicate of astral-sh/ty#199:

In general, symbols imported into stub files are not considered "re-exported" by that stub file; they are considered "private to the stub".

In other words, if a stub file has this:

```py
# foo.pyi
from typing import Iterable
```

the type checker is meant to disallow this:

```py
from foo import Iterable
```

_However_, there are several exceptions to this:
1. If an import uses the "redundant alias convention", it is considered "re-exported" from a stub file (`from typing import Iterable as iterable`)
2. If a symbol is imported into a stub file using a `*` import, it is considered re-exported from the stub file
3. If a symbol is included in `__all__`, it is considered re-exported from the stub file.

We understand exceptions (1) and (2) currently, but not exception (3), which is astral-sh/ty#199. The symbol `sha256` is only re-exported from `hashlib.pyi` in typeshed (the canonical stub file for the stdlib `hashlib` module) due to exception (3).

---

_Comment by @AlexWaygood on 2025-05-03 14:40_

References:
- Typing spec docs on the re-export convention: https://typing.python.org/en/latest/spec/distributing.html#library-interface-public-and-private-symbols
- Definition of `sha256` in our vendored typeshed stub for `hashlib`. It does not use a "redundant alias" or a `*` import to define the symbol, so neither exception (1) nor exception (2) applies: https://github.com/astral-sh/ruff/blob/91481a8be75446c7b404211fdeccd65c68a371ff/crates/red_knot_vendored/vendor/typeshed/stdlib/hashlib.pyi#L3-L20
- `__all__` definition in our vendored typeshed stub for `hashlib` (which includes `sha256`): https://github.com/astral-sh/ruff/blob/91481a8be75446c7b404211fdeccd65c68a371ff/crates/red_knot_vendored/vendor/typeshed/stdlib/hashlib.pyi#L26-L46

---

_Comment by @dhruvmanila on 2025-05-03 14:51_

This should be fixed soon, I'm working on it.

---
