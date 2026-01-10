---
number: 15027
title: "Import-related mdtests aren't isolated"
type: issue
state: closed
author: dcreager
labels:
  - bug
  - testing
  - ty
assignees: []
created_at: 2024-12-16T23:27:53Z
updated_at: 2024-12-17T11:45:38Z
url: https://github.com/astral-sh/ruff/issues/15027
synced_at: 2026-01-10T01:22:55Z
---

# Import-related mdtests aren't isolated

---

_Issue opened by @dcreager on 2024-12-16 23:27_

When testing #15026 I got spurious test failures that look like the different test cases within an mdtest file aren't as isolated as they should be:

At commit 65da13900cd07c365159a67a6e1cda6442030e2f, the `Relative - Dotted` test fails when you test the entire file:

```
$ cargo test -p red_knot_python_semantic -- mdtest__import_relative
[snip]
relative.md - Relative - Dotted
  crates/red_knot_python_semantic/resources/mdtest/import/relative.md:39 unexpected error: [unresolved-import] "Cannot resolve import `.foo.bar.baz`"
  crates/red_knot_python_semantic/resources/mdtest/import/relative.md:41 unmatched assertion: revealed: Literal[42]
  crates/red_knot_python_semantic/resources/mdtest/import/relative.md:41 unexpected error: 1 [revealed-type] "Revealed type is `Unknown`"
```

but pass when you run that test by itself:

```
$ env MDTEST_TEST_FILTER="relative.md - Relative - Dotted" cargo test -p red_knot_python_semantic -- mdtest__import_relative
```

Renaming the files in that one test case to be unique within the mdtest file (c5fd3c64ae94e937fb3bd2c913830d8cb246df74) also causes test to pass for both invocations.

---

_Label `red-knot` added by @dcreager on 2024-12-16 23:27_

---

_Label `testing` added by @AlexWaygood on 2024-12-16 23:31_

---

_Label `bug` added by @AlexWaygood on 2024-12-16 23:31_

---

_Comment by @MichaReiser on 2024-12-17 07:51_

I haven't figured out the failure yet but I think the test failing is the *expected* output because the `__init__.py` files are missing for the `bar` and `foo` package:

````
```toml
log = true
```

```py path=package/__init__.py
```

```py path=package/foo/__init__.py
```

```py path=package/foo/bar/__init__.py
```

```py path=package/foo/bar/baz.py
X = 42
```

```py path=package/bar.py
from .foo.bar.baz import X

reveal_type(X)  # revealed: Literal[42]
```
````

Edit: Never mind, I guess it is a namespace package?

---

_Referenced in [astral-sh/ruff#15030](../../astral-sh/ruff/pulls/15030.md) on 2024-12-17 08:27_

---

_Closed by @MichaReiser on 2024-12-17 11:45_

---

_Closed by @MichaReiser on 2024-12-17 11:45_

---
