```yaml
number: 1734
title: Improve diagnostics for failed reverse resolution (file-to-module) and failed submodule resolution with namespace packages
type: issue
state: open
author: winterqt
labels:
  - diagnostics
  - imports
assignees: []
created_at: 2025-12-02T23:47:46Z
updated_at: 2025-12-03T19:02:13Z
url: https://github.com/astral-sh/ty/issues/1734
synced_at: 2026-01-12T15:54:25Z
```

# Improve diagnostics for failed reverse resolution (file-to-module) and failed submodule resolution with namespace packages

---

_@winterqt_

### Summary

Given the following directory structure:
```
nixos/lib/test-driver/src
├── extract-docstrings.py
├── pyproject.toml
└── test_driver
    ├── debug.py
    ├── driver.py
    ├── errors.py
    ├── __init__.py
    ├── logger.py
    ├── machine
    │   ├── __init__.py
    │   ├── ocr.py
    │   └── qmp.py
    ├── polling_condition.py
    ├── py.typed
    └── vlan.py
```
(ty is given access to just this directory, as `/build/src`, nothing else is relevant)

`ty` fails to resolve imports from `machine/__init__.py`: https://gist.github.com/winterqt/3f554709d2ba72999acc70714570535a

I've tried changing the name of this directory from `src`, and also adding `./test_driver` as a root. Curiously enough, if I add `./test_driver/machine` as a root, I get a hint telling me to try removing the `.` from the imports, so ty can detect them properly in some instances.

I'm not sure why "could not resolve file `/build/src/test_driver/machine/__init__.py` to a module (try adjusting configured search paths?)" would be happening given the above working case, so entirely possible this is user error.

### Version

ty 0.0.1-alpha.29

---

_Label `imports` added by @carljm on 2025-12-03 00:16_

---

_Label `needs-mre` added by @carljm on 2025-12-03 00:17_

---

_Comment by @carljm on 2025-12-03 00:24_

Hmm, this is very strange. Spent a while looking at those logs and it seems like this should definitely work, and when I construct the "same" situation locally, those relative imports work fine. So there is some factor here I'm not seeing. I'm suspecting maybe something with symlinks, given the `nixos/lib/test-driver/src` which ty is "given access to" as `build/src`? I checked the logs and don't see discrepancies (everything seems to be based on `build/src/`) but maybe internally we're sometimes canonicalizing a path and sometimes not?

The other thing I wonder is whether all those `PYTHONPATH` entries could be causing a problem somehow? Would it be easy for you to try running ty without `PYTHONPATH` set?

---

_Comment by @winterqt on 2025-12-03 00:50_

Appreciate you taking a look!

To clarify: `nixos/lib/test-driver/src` is simply copied to a sandbox, so ty sees everything in it with the path `/build/src` (I provided the detail just to clear up why the path in my tree was different).

Yeah, I can try it without `PYTHONPATH` tomorrow.

---

_Comment by @MichaReiser on 2025-12-03 06:37_

I think the issue here is that test_driver isn't a valid module name. This breaks file_to_module, which we use to resolve relative imports. 

I would expect that adding src/test_driver makes ty understand the relative imports. 

Cc @Gankra 

---

_Comment by @carljm on 2025-12-03 06:43_

> I think the issue here is that test_driver isn't a valid module name.

But `test_driver` is a valid module name. Underscores are valid characters in module names.

> I would expect that adding src/test_driver makes ty understand the relative imports.

I don't understand this, there is no `src/test_driver` in the given directory layout. And OP already mentioned that adding `test_driver/` as a root doesn't fix the problem.

---

_Comment by @MichaReiser on 2025-12-03 08:21_

Right, `_` are allowed in module names, it's only `-` that isn't.


The important line from the logs is:

```
nixos-test-driver>   2025-12-02T23:42:55.708961Z DEBUG ty_python_semantic::types::infer::builder: Relative module resolution `.ocr` failed: could not resolve file `/build/src/test_driver/machine/__init__.py` to a module (try adjusting configured search paths?)
```

We, unfortunately, don't log why that is. Looking at the `PYTHON_PATH`, the following seems suspicious to me:

```
nixos-test-driver>   2025-12-02T23:42:55.690158Z DEBUG ruff_db::files::file_root: Adding new file root '/nix/store/y6mwh1infmv0hdi5pkflqybsqwfp4dii-nixos-test-driver-1.1/lib/python3.13/site-packages' of kind LibrarySearchPath
nixos-test-driver>     at crates/ruff_db/src/files/file_root.rs:84 on ThreadId(1)
nixos-test-driver>
```

Is this your own `test-driver` package? If so, what I think is happening is that ty's `file_to_module` returns `none` because `machine` resolves to the module in `y6mwh1infmv0hdi5pkflqybsqwfp4dii-nixos-test-driver-1.1` and not to the first party one. 

---

_Comment by @Gankra on 2025-12-03 13:43_

Hmm I think we should start surfacing more end-user diagnostics here.

---

_Comment by @Gankra on 2025-12-03 14:07_

Common failure modes that I believe we're bad at surfacing:

* `.` expansion (`file_to_module` making `.` into an absolute module name)
  * What search-path matches were rejected for invalid module names?
  * What matches were rejected because another module already claimed the ModuleName?
  * If it succeeded, what absolute module name was computed, and because of what search-path (in case the answer is surprising)
* Module resolution: what partial matches were found? 
  * Did we find a non-namespace module that didn't have the next component? (causing immediate termination)
  * Did we find a namespace module that didn't have the next component?

---

_Comment by @winterqt on 2025-12-03 16:13_

With `PYTHONPATH=`, it looks like it's working:
```
2025-12-03 16:12:55.066176077 DEBUG Version: 0.0.1-alpha.29
2025-12-03 16:12:55.066288929 DEBUG Architecture: x86_64, OS: linux, case-sensitive: case-sensitive
2025-12-03 16:12:55.066331499 DEBUG Searching for a project in '/build/src'
2025-12-03 16:12:55.066424024 DEBUG Project without `tool.ty` section: '/build/src'
2025-12-03 16:12:55.066435696 DEBUG Searching for a user-level configuration at `/homeless-shelter/.config/ty/ty.toml`
2025-12-03 16:12:55.067368892 INFO Defaulting to python-platform `linux`
2025-12-03 16:12:55.067384091 DEBUG Discovering virtual environment in `/build/src`
2025-12-03 16:12:55.067414719 DEBUG Attempting to parse virtual environment metadata at '/nix/store/klrwdfngp0385xdqmmgc3pf5mbdwsj42-ty-0.0.1-alpha.29/pyvenv.cfg'
2025-12-03 16:12:55.067423535 DEBUG Failed to discover ty's environment: Invalid ty environment `/nix/store/klrwdfngp0385xdqmmgc3pf5mbdwsj42-ty-0.0.1-alpha.29`: points to a broken venv with no pyvenv.cfg file
2025-12-03 16:12:55.0674315 DEBUG No virtual environment found
2025-12-03 16:12:55.067442601 DEBUG Including `.` in `environment.root`
2025-12-03 16:12:55.067455536 DEBUG Adding `/build/src` from the `PYTHONPATH` environment variable to `extra_paths`
2025-12-03 16:12:55.067464212 DEBUG Adding extra search-path `/build/src`
2025-12-03 16:12:55.067471425 DEBUG Adding first-party search path `/build/src`
2025-12-03 16:12:55.067478629 DEBUG Using vendored stdlib
2025-12-03 16:12:55.067715936 INFO Python version: Python 3.14, platform: linux
2025-12-03 16:12:55.067727778 DEBUG Adding new file root '/build/src' of kind LibrarySearchPath
2025-12-03 16:12:55.067947021 DEBUG Starting main loop
2025-12-03 16:12:55.067961588 DEBUG Waiting for next main loop message.
2025-12-03 16:12:55.06800003 DEBUG Checking all files in project 'nixos-test-driver'
2025-12-03 16:12:55.069707484 INFO Indexed 21 file(s) in 0.002s
2025-12-03 16:12:55.069888014 DEBUG Checking file '/build/src/test_driver/errors.py'
2025-12-03 16:12:55.06988575 DEBUG Checking file '/build/src/build/lib/test_driver/errors.py'
2025-12-03 16:12:55.069959639 DEBUG Checking file '/build/src/test_driver/debug.py'
2025-12-03 16:12:55.069972874 DEBUG Checking file '/build/src/test_driver/polling_condition.py'
2025-12-03 16:12:55.069985197 DEBUG Checking file '/build/src/test_driver/machine/ocr.py'
2025-12-03 16:12:55.069983855 DEBUG Checking file '/build/src/build/lib/test_driver/vlan.py'
2025-12-03 16:12:55.070019021 DEBUG Checking file '/build/src/test_driver/machine/qmp.py'
2025-12-03 16:12:55.070023469 DEBUG Checking file '/build/src/build/lib/test_driver/machine/qmp.py'
2025-12-03 16:12:55.070057824 DEBUG Checking file '/build/src/test_driver/__init__.py'
2025-12-03 16:12:55.069987411 DEBUG Checking file '/build/src/extract-docstrings.py'
2025-12-03 16:12:55.069990467 DEBUG Checking file '/build/src/build/lib/test_driver/polling_condition.py'
2025-12-03 16:12:55.070062383 DEBUG Checking file '/build/src/build/lib/test_driver/machine/ocr.py'
2025-12-03 16:12:55.070141121 DEBUG Checking file '/build/src/build/lib/test_driver/debug.py'
2025-12-03 16:12:55.070144166 DEBUG Checking file '/build/src/build/lib/test_driver/__init__.py'
2025-12-03 16:12:55.069983354 DEBUG Checking file '/build/src/test_driver/vlan.py'
2025-12-03 16:12:55.070631604 DEBUG Checking file '/build/src/test_driver/logger.py'
2025-12-03 16:12:55.071169366 DEBUG Checking file '/build/src/build/lib/test_driver/driver.py'
2025-12-03 16:12:55.071671091 DEBUG Checking file '/build/src/test_driver/driver.py'
2025-12-03 16:12:55.071864705 DEBUG Checking file '/build/src/build/lib/test_driver/logger.py'
2025-12-03 16:12:55.072043772 DEBUG Checking file '/build/src/build/lib/test_driver/machine/__init__.py'
2025-12-03 16:12:55.072619426 DEBUG Checking file '/build/src/test_driver/machine/__init__.py'
2025-12-03 16:12:55.076285506 DEBUG Resolving dynamic module resolution paths
2025-12-03 16:12:55.07630854 DEBUG Module `ptpython.ipython` not found in search paths
2025-12-03 16:12:55.076313479 DEBUG Module `remote_pdb` not found in search paths
2025-12-03 16:12:55.089025378 DEBUG Module `colorama` not found in search paths
2025-12-03 16:12:55.08914879 DEBUG Module `junit_xml` not found in search paths
2025-12-03 16:12:55.100782118 DEBUG File `/build/src/test_driver/errors.py` was reparsed after being collected in the current Salsa revision
2025-12-03 16:12:55.105061753 DEBUG File `/build/src/test_driver/debug.py` was reparsed after being collected in the current Salsa revision
2025-12-03 16:12:55.105125433 DEBUG File `/build/src/test_driver/debug.py` was reparsed after being collected in the current Salsa revision
2025-12-03 16:12:55.106095519 DEBUG File `/build/src/test_driver/vlan.py` was reparsed after being collected in the current Salsa revision
2025-12-03 16:12:55.119069441 DEBUG File `/build/src/build/lib/test_driver/machine/qmp.py` was reparsed after being collected in the current Salsa revision
2025-12-03 16:12:55.119663479 DEBUG File `/build/src/test_driver/machine/qmp.py` was reparsed after being collected in the current Salsa revision
2025-12-03 16:12:55.120109699 DEBUG File `/build/src/test_driver/polling_condition.py` was reparsed after being collected in the current Salsa revision
2025-12-03 16:12:55.130774534 DEBUG File `/build/src/build/lib/test_driver/machine/ocr.py` was reparsed after being collected in the current Salsa revision
2025-12-03 16:12:55.130827995 DEBUG File `/build/src/test_driver/machine/ocr.py` was reparsed after being collected in the current Salsa revision
2025-12-03 16:12:55.136482639 DEBUG Checking all files took 0.067s
error[unresolved-import]: Cannot resolve imported module `ptpython.ipython`
 --> build/lib/test_driver/__init__.py:6:8
  |
4 | from pathlib import Path
5 |
6 | import ptpython.ipython
  |        ^^^^^^^^^^^^^^^^
7 |
8 | from test_driver.debug import Debug, DebugAbstract, DebugNop
  |
info: Searched in the following paths during module resolution:
info:   1. /build/src (extra search path specified on the CLI or in your config file)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

error[unresolved-import]: Cannot resolve imported module `colorama`
  --> build/lib/test_driver/driver.py:14:6
   |
12 | from unittest import TestCase
13 |
14 | from colorama import Style
   |      ^^^^^^^^
15 |
16 | from test_driver.debug import DebugAbstract, DebugNop
   |
info: Searched in the following paths during module resolution:
info:   1. /build/src (extra search path specified on the CLI or in your config file)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

error[unresolved-import]: Cannot resolve imported module `colorama`
  --> build/lib/test_driver/logger.py:16:6
   |
14 | from xml.sax.xmlreader import AttributesImpl
15 |
16 | from colorama import Fore, Style
   |      ^^^^^^^^
17 | from junit_xml import TestCase, TestSuite
   |
info: Searched in the following paths during module resolution:
info:   1. /build/src (extra search path specified on the CLI or in your config file)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

error[unresolved-import]: Cannot resolve imported module `junit_xml`
  --> build/lib/test_driver/logger.py:17:6
   |
16 | from colorama import Fore, Style
17 | from junit_xml import TestCase, TestSuite
   |      ^^^^^^^^^
   |
info: Searched in the following paths during module resolution:
info:   1. /build/src (extra search path specified on the CLI or in your config file)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

error[unresolved-attribute]: Object of type `() -> bool | None` has no attribute `__name__`
  --> build/lib/test_driver/polling_condition.py:36:36
   |
34 |                 self.description = condition.__doc__
35 |             else:
36 |                 self.description = condition.__name__
   |                                    ^^^^^^^^^^^^^^^^^^
37 |         else:
38 |             self.description = str(description)
   |
info: rule `unresolved-attribute` is enabled by default

error[unresolved-import]: Cannot resolve imported module `ptpython.ipython`
 --> test_driver/__init__.py:6:8
  |
4 | from pathlib import Path
5 |
6 | import ptpython.ipython
  |        ^^^^^^^^^^^^^^^^
7 |
8 | from test_driver.debug import Debug, DebugAbstract, DebugNop
  |
info: Searched in the following paths during module resolution:
info:   1. /build/src (extra search path specified on the CLI or in your config file)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

error[unresolved-import]: Cannot resolve imported module `colorama`
  --> test_driver/driver.py:14:6
   |
12 | from unittest import TestCase
13 |
14 | from colorama import Style
   |      ^^^^^^^^
15 |
16 | from test_driver.debug import DebugAbstract, DebugNop
   |
info: Searched in the following paths during module resolution:
info:   1. /build/src (extra search path specified on the CLI or in your config file)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

error[unresolved-import]: Cannot resolve imported module `colorama`
  --> test_driver/logger.py:16:6
   |
14 | from xml.sax.xmlreader import AttributesImpl
15 |
16 | from colorama import Fore, Style
   |      ^^^^^^^^
17 | from junit_xml import TestCase, TestSuite
   |
info: Searched in the following paths during module resolution:
info:   1. /build/src (extra search path specified on the CLI or in your config file)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

error[unresolved-import]: Cannot resolve imported module `junit_xml`
  --> test_driver/logger.py:17:6
   |
16 | from colorama import Fore, Style
17 | from junit_xml import TestCase, TestSuite
   |      ^^^^^^^^^
   |
info: Searched in the following paths during module resolution:
info:   1. /build/src (extra search path specified on the CLI or in your config file)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

error[unresolved-attribute]: Object of type `() -> bool | None` has no attribute `__name__`
  --> test_driver/polling_condition.py:36:36
   |
34 |                 self.description = condition.__doc__
35 |             else:
36 |                 self.description = condition.__name__
   |                                    ^^^^^^^^^^^^^^^^^^
37 |         else:
38 |             self.description = str(description)
   |
info: rule `unresolved-attribute` is enabled by default

Found 10 diagnostics
2025-12-03 16:12:55.137098468 DEBUG Exiting main loop
```

---

_Comment by @carljm on 2025-12-03 18:58_

Thanks all for the debugging! I'm going to close this as a duplicate of https://github.com/astral-sh/ty/issues/1682 because I think the proposed fix there would have eliminated this problem. First-party code should come before `PYTHONPATH` entries in the search path.

---

_Closed by @carljm on 2025-12-03 18:58_

---

_Comment by @carljm on 2025-12-03 19:00_

Or maybe it would be useful to have a separate ticket for improving our diagnostics / user feedback in the cases @Gankra mentions? We could dedicate this ticket to that.

---

_Reopened by @carljm on 2025-12-03 19:00_

---

_Added to milestone `Stable` by @carljm on 2025-12-03 19:00_

---

_Label `needs-mre` removed by @carljm on 2025-12-03 19:00_

---

_Renamed from "Relative imports not being resolved" to "Improve diagnostics for failed reverse resolution (file-to-module) and failed submodule resolution with namespace packages" by @carljm on 2025-12-03 19:02_

---

_Label `diagnostics` added by @carljm on 2025-12-03 19:02_

---
