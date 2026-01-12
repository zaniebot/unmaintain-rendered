```yaml
number: 487
title: improve diagnostics for C extension modules without stubs
type: issue
state: open
author: johnniemorrow
labels:
  - diagnostics
  - imports
assignees: []
created_at: 2025-05-22T13:57:20Z
updated_at: 2025-11-24T17:16:48Z
url: https://github.com/astral-sh/ty/issues/487
synced_at: 2026-01-12T15:54:23Z
```

# improve diagnostics for C extension modules without stubs

---

_@johnniemorrow_

### Summary

Hi ty-team,

I was trying to fix up some unresolved-attribute errors reported by ty related to lxml.etree in [lxml](https://pypi.org/project/lxml/).

Reducing it down to just an import statement, ty doesn't seem to be able to see lxml.etree:

```bash
$ cat use_lxml.py 
import lxml.etree

```


```bash
$ ty check use_lxml.py 
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unresolved-import]: Cannot resolve imported module `lxml.etree`
 --> use_lxml.py:1:8
  |
1 | import lxml.etree
  |        ^^^^^^^^^^
  |
info: make sure your Python environment is properly configured: https://github.com/astral-sh/ty/blob/main/docs/README.md#python-environment
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic

``` 

### Version

_No response_

---

_Label `imports` added by @AlexWaygood on 2025-05-22 13:58_

---

_Comment by @sharkdp on 2025-05-22 14:10_

Thank you for reporting this.

`lxml.etree` is a C extension module. I wonder if we could detect this and show a better error?

You probably want to fix this by installing `types-lxml`?

---

_Comment by @AlexWaygood on 2025-05-22 14:15_

> `lxml.etree` is a C extension module. I wonder if we could detect this and show a better error?

Pyright does a fancy thing in its module resolver to distinguish between modules that are definitely unresolvable (no python or C-extension modules are available) and modules which might be resolvable but which it cannot inspect, because it's a C extension or similar and there are no stubs installed.

Mypy does not have that feature, but it does have a hardcoded list of untyped packages for which it knows high-quality stubs are available, and issues a nice diagnostic suggesting to install the relevant stubs package if it issues an `unresolved-import` diagnostic on one of those packages.

We don't have either feature right now; we could consider adding one or both. (The mypy one is easier to implement!)

---

_Comment by @johnniemorrow on 2025-05-22 14:49_

> Thank you for reporting this.
> 
> `lxml.etree` is a C extension module. I wonder if we could detect this and show a better error?
> 
> You probably want to fix this by installing `types-lxml`?

`uv add types-lxml --dev` does the trick - thanks @sharkdp!

---

_Renamed from "cannot resolve lxml.etree" to "improve diagnostics for C extension modules without stubs" by @carljm on 2025-05-22 14:56_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-22 14:58_

---

_Comment by @AlexWaygood on 2025-06-27 20:41_

#715 includes some discussion on exactly which extensions should be recognized when resolving modules to C extensions, and some other details of the implementation.

---

_Added to milestone `Stable` by @carljm on 2025-11-14 00:20_

---

_Comment by @carljm on 2025-11-24 17:16_

#1613 raises the same scenario, and also suggests that we might be able to automatically introspect the `.so` and generate some implicit default stubs automatically.

---
