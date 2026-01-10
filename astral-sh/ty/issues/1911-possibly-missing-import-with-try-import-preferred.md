```yaml
number: 1911
title: "possibly-missing-import with try: import preferred; except ImportError: import alternative"
type: issue
state: closed
author: emeralddcw
labels:
  - imports
  - control flow
assignees: []
created_at: 2025-12-15T22:20:23Z
updated_at: 2025-12-17T23:46:38Z
url: https://github.com/astral-sh/ty/issues/1911
synced_at: 2026-01-10T01:53:59Z
```

# possibly-missing-import with try: import preferred; except ImportError: import alternative

---

_Issue opened by @emeralddcw on 2025-12-15 22:20_

### Summary

**Description**
`ty` reports a `probably-missing-import` on an import line that is wrapped in an `try..except ImportError` block. Since the except block makes the same imported class name available, I did not expect the missing import warning.

**Reproducer**
The following example is derived from the [pyyaml documentation](https://pyyaml.org/wiki/PyYAMLDocumentation).

```python
from yaml import load, dump
import sys

try:
    from yaml import CLoader as Loader, CDumper as Dumper
except ImportError:
    from yaml import Loader, Dumper

data = load(sys.stdin, Loader=Loader)
print(dump(data, Dumper=Dumper))
```

Note: I also tried with `except (ImportError, ModuleNotFoundError)` and `except Exception`, getting the same outcome both cases.

```
% uvx --with pyyaml ty check
warning[possibly-missing-import]: Member `CLoader` of module `yaml` may be missing
 --> repro.py:5:22
  |
4 | try:
5 |     from yaml import CLoader as Loader, CDumper as Dumper
  |                      ^^^^^^^^^^^^^^^^^
6 | except ImportError:
7 |     from yaml import Loader, Dumper
  |
info: rule `possibly-missing-import` is enabled by default

warning[possibly-missing-import]: Member `CDumper` of module `yaml` may be missing
 --> repro.py:5:41
  |
4 | try:
5 |     from yaml import CLoader as Loader, CDumper as Dumper
  |                                         ^^^^^^^^^^^^^^^^^
6 | except ImportError:
7 |     from yaml import Loader, Dumper
  |
info: rule `possibly-missing-import` is enabled by default

Found 2 diagnostics
```

### Version

ty 0.0.1-alpha.34 (ef3d48ac4 2025-12-12)

---

_Comment by @carljm on 2025-12-16 02:00_

Thanks for the report!

In general, Python type checkers don't currently ever silence diagnostics based on try/except blocks. If there were a case for doing so, it would be this example (and maybe `try: d["key"]; except IndexError: ...`).

ty does differ from some other type checkers in that we emit `possibly-missing-import` at all; other type checkers will just assume that a possibly-undefined import is always defined. So you can easily get similar behavior to other type checkers in this case by just silencing the `possibly-missing-import` rule in your configuration. Or you can add `# ty: ignore[possibly-missing-import]` on this line. Or (best option?) you can install the `types-PyYaml` package, which has type stubs for pyyaml which also pretend that `CLoader` and `CDumper` always exist.

All that said, there are similar cases where the attempted import may not exist at all (in the environment we are type checking against), and it might still be nice to silence errors that we know are "caught" (and/or consider try/except ImportError to be a statically-known control flow, based on what modules we see exist). I don't think this will be an immediate priority, but it is something we could consider.

---

_Label `control flow` added by @carljm on 2025-12-16 02:01_

---

_Label `imports` added by @carljm on 2025-12-16 02:01_

---

_Comment by @emeralddcw on 2025-12-16 18:44_

Thanks for the explanation about how this works, where it may/may-not fit in the project roadmap, and the suggestions for ways to move forward with my usage of ty. I missed that stubs package, and that sounds like a usable solution for my case. I should probably figure out a way to scrub for other missing stubs packages that cover my dependencies.

---

_Closed by @carljm on 2025-12-17 23:46_

---
