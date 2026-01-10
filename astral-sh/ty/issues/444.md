```yaml
number: 444
title: "[panic] Panic 'too many cycle iterations' for recursive star import"
type: issue
state: closed
author: anatoli-tsinovoy
labels:
  - fatal
  - imports
assignees: []
created_at: 2025-05-19T08:45:21Z
updated_at: 2025-11-13T14:38:11Z
url: https://github.com/astral-sh/ty/issues/444
synced_at: 2026-01-10T02:06:24Z
```

# [panic] Panic 'too many cycle iterations' for recursive star import

---

_Issue opened by @anatoli-tsinovoy on 2025-05-19 08:45_

Hi, I'm experiencing the following panic when trying to use [construct](https://github.com/construct/construct), ([pypi](https://pypi.org/project/construct/)).

Attached is an MRE and the output of `RUST_BACKTRACE=1 uvx ty check mre.py  -vv`.

Issue appears strongly related to #386 , possibly it has something to do with the multiple `*` imports used in the library?
~~Also looks somewhat related to #380  and #256 , though at a glance I don't see any relation to generics.~~


Let me know if there's anything else I can provide to help. 🙏🏻 

<details>

## File: MRE/mre.py
```python
from io import BytesIO

from construct import Float32l


def main():
    dummy_io = BytesIO(b"\x00\x00\x00\x00")
    print(Float32l.parse_stream(dummy_io))


if __name__ == "__main__":
    main()

```

## File: MRE/uv.lock
```toml
version = 1
revision = 2
requires-python = "==3.13.*"

[[package]]
name = "construct"
version = "2.10.70"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/02/77/8c84b98eca70d245a2a956452f21d57930d22ab88cbeed9290ca630cf03f/construct-2.10.70.tar.gz", hash = "sha256:4d2472f9684731e58cc9c56c463be63baa1447d674e0d66aeb5627b22f512c29", size = 86337, upload-time = "2023-11-29T08:44:49.545Z" }
wheels = [
    { url = "https://files.pythonhosted.org/packages/b2/fb/08b3f4bf05da99aba8ffea52a558758def16e8516bc75ca94ff73587e7d3/construct-2.10.70-py3-none-any.whl", hash = "sha256:c80be81ef595a1a821ec69dc16099550ed22197615f4320b57cc9ce2a672cb30", size = 63020, upload-time = "2023-11-29T08:44:46.876Z" },
]

[[package]]
name = "construct-mre"
version = "0.1.0"
source = { virtual = "." }
dependencies = [
    { name = "construct" },
]

[package.metadata]
requires-dist = [{ name = "construct" }]

```

## File: MRE/pyproject.toml
```toml
[project]
name = "construct-mre"
version = "0.1.0"
requires-python = ">=3.13,<3.14"
dependencies = ["construct"]
[tool.ruff]
target-version = "py313"
```

```
$ uv sync
$ RUST_BACKTRACE=1 uvx ty check mre.py  -vv
2025-05-19 11:28:15.152214493 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
2025-05-19 11:28:15.156542092 DEBUG Version: 0.0.1-alpha.5
2025-05-19 11:28:15.156558632 DEBUG Architecture: x86_64, OS: linux, case-sensitive: case-sensitive
2025-05-19 11:28:15.156619544 DEBUG Searching for a project in '/root/MRE'
2025-05-19 11:28:15.156710746 DEBUG Resolving requires-python constraint: `>=3.13, <3.14`
2025-05-19 11:28:15.156720586 DEBUG Resolved requires-python constraint to: 3.13
2025-05-19 11:28:15.156819628 DEBUG Resolving requires-python constraint: `>=3.13, <3.14`
2025-05-19 11:28:15.156826348 DEBUG Resolved requires-python constraint to: 3.13
2025-05-19 11:28:15.156830298 DEBUG Found project at '/'
2025-05-19 11:28:15.156844038 DEBUG Searching for a user-level configuration at `/root/.config/ty/ty.toml`
2025-05-19 11:28:15.156887549 INFO Defaulting to python-platform `linux`
2025-05-19 11:28:15.15690396 INFO Python version: Python 3.13, platform: linux
2025-05-19 11:28:15.15691343 DEBUG Adding first-party search path '/'
2025-05-19 11:28:15.15693106 DEBUG Using vendored stdlib
2025-05-19 11:28:15.15741041 DEBUG Discovering site-packages paths from sys-prefix `/.venv` (`VIRTUAL_ENV` environment variable')
2025-05-19 11:28:15.15742474 DEBUG Attempting to parse virtual environment metadata at '/.venv/pyvenv.cfg'
2025-05-19 11:28:15.157459671 DEBUG Searching for site-packages directory in `sys.prefix` path `/.venv`
2025-05-19 11:28:15.157469611 DEBUG Resolved site-packages directories for this virtual environment are: ["/.venv/lib/python3.13/site-packages"]
2025-05-19 11:28:15.157479201 DEBUG Adding site-packages search path '/.venv/lib/python3.13/site-packages'
2025-05-19 11:28:15.157578523 DEBUG Setting included paths: 1
2025-05-19 11:28:15.157589554 DEBUG Reloading files for project `***`
2025-05-19 11:28:15.157637385 DEBUG Starting main loop
2025-05-19 11:28:15.157645985 DEBUG Waiting for next main loop message.
2025-05-19 11:28:15.157674185 DEBUG Checking project '***'
2025-05-19 11:28:15.159653446 INFO Indexed 1 file(s)
Checking ------------------------------------------------------------ 0/1 files                                                             2025-05-19 11:28:15.159798329 DEBUG Checking file '/root/MRE/mre.py'
2025-05-19 11:28:15.161136276 DEBUG Resolving dynamic module resolutio
error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/7edce6e/src/function/execute.rs:209:25 when checking `/root/MRE/mre.py`: `exported_names(Id(2017)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Args: ["ty", "check", "mre.py", "-vv"]
info: Backtrace:
   0: <unknown>
   1: <unknown>
   2: <unknown>
   3: <unknown>
   4: <unknown>
   5: <unknown>
   6: <unknown>
   7: <unknown>
   8: <unknown>
   9: <unknown>
  10: <unknown>
  11: <unknown>
  12: <unknown>
  13: <unknown>
  14: <unknown>
  15: <unknown>
  16: <unknown>
  17: <unknown>
  18: <unknown>
  19: <unknown>
  20: <unknown>
  21: <unknown>
  22: <unknown>
  23: <unknown>
  24: <unknown>
  25: <unknown>
  26: <unknown>
  27: <unknown>
  28: <unknown>
  29: <unknown>
  30: <unknown>
  31: <unknown>
  32: <unknown>
  33: <unknown>
  34: <unknown>
  35: <unknown>
  36: <unknown>
  37: <unknown>
  38: <unknown>
  39: <unknown>
  40: <unknown>
  41: <unknown>
  42: <unknown>
  43: <unknown>
  44: <unknown>
  45: <unknown>
  46: <unknown>
  47: <unknown>
  48: <unknown>
  49: <unknown>
  50: <unknown>
  51: <unknown>
  52: <unknown>
  53: <unknown>
  54: <unknown>
  55: <unknown>
  56: <unknown>
  57: <unknown>
  58: <unknown>
  59: <unknown>
  60: <unknown>
  61: <unknown>
  62: <unknown>
  63: <unknown>
  64: <unknown>
  65: <unknown>
  66: <unknown>
  67: <unknown>
  68: <unknown>
  69: <unknown>
  70: <unknown>
  71: <unknown>
  72: <unknown>
  73: <unknown>
  74: <unknown>
  75: <unknown>
  76: <unknown>
  77: <unknown>
  78: <unknown>
  79: <unknown>
  80: <unknown>
  81: <unknown>
  82: <unknown>
  83: <unknown>
  84: clone

info: query stacktrace:
   0: exported_names(Id(202c))
             at crates/ty_python_semantic/src/semantic_index/re_exports.rs:46
   1: exported_names(Id(2020))
             at crates/ty_python_semantic/src/semantic_index/re_exports.rs:46
   2: exported_names(Id(201e))
             at crates/ty_python_semantic/src/semantic_index/re_exports.rs:46
   3: semantic_index(Id(2017))
             at crates/ty_python_semantic/src/semantic_index.rs:49
   4: global_scope(Id(2017))
             at crates/ty_python_semantic/src/semantic_index.rs:138
   5: member_lookup_with_policy_(Id(2802))
             at crates/ty_python_semantic/src/types.rs:536
   6: infer_definition_types(Id(1401))
             at crates/ty_python_semantic/src/types/infer.rs:148
   7: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:121
   8: check_types(Id(c00))
             at crates/ty_python_semantic/src/types.rs:84


Found 1 diagnostic
2025-05-19 11:28:15.17109034 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
2025-05-19 11:28:15.17109648 DEBUG Exiting main loop
```

</details>

---

_Label `fatal` added by @AlexWaygood on 2025-05-19 12:14_

---

_Label `imports` added by @AlexWaygood on 2025-05-19 12:14_

---

_Comment by @sharkdp on 2025-05-19 12:57_

Thank you for reporting this!

> Issue appears strongly related to [#386](https://github.com/astral-sh/ty/issues/386) , possibly it has something to do with the multiple `*` imports used in the library?

Yes, indeed. I managed to minify it to the following MRE, which still involves five files (all originally from `construct`). There is an obvious cycle there (binary -> construct -> core -> lib -> binary), but the panic goes away if any of the statements here are removed (I have not investigated further).  If some statements are removed (like `class Mapping(Adapter): ...` in `core.py`), it leads to a non-deterministic hang.

```py
─────────────────────────────────────────────────────────────────
# File: binary.py
─────────────────────────────────────────────────────────────────
from construct import *
─────────────────────────────────────────────────────────────────

─────────────────────────────────────────────────────────────────
# File: construct.py
─────────────────────────────────────────────────────────────────
from core import *
─────────────────────────────────────────────────────────────────

─────────────────────────────────────────────────────────────────
# File: core.py
─────────────────────────────────────────────────────────────────
from lib import *


class Mapping(Adapter): ...
─────────────────────────────────────────────────────────────────

─────────────────────────────────────────────────────────────────
# File: lib.py
─────────────────────────────────────────────────────────────────
from binary import *
from py3compat import *
─────────────────────────────────────────────────────────────────

─────────────────────────────────────────────────────────────────
# File: py3compat.py
─────────────────────────────────────────────────────────────────
import sys, platform

PYPY = "__pypy__" in sys.builtin_module_names
stringtypes = (
    bytes,
    str,
)
integertypes = (int,)
unicodestringtype = str
─────────────────────────────────────────────────────────────────
```

---

_Renamed from "[panic] Panic due to 'too many cycle iterations' when importing `construct`" to "[panic] Panic 'too many cycle iterations' for recursive star import" by @sharkdp on 2025-05-19 12:59_

---

_Comment by @MichaReiser on 2025-05-19 13:40_

> it leads to a non-deterministic hang.

Is this with parallelism or without?

---

_Comment by @sharkdp on 2025-05-19 14:20_

> > it leads to a non-deterministic hang.
> 
> Is this with parallelism or without?

Only hangs with parallelism. Succeeds single-threaded without any error for all 5! = 120 possible permutations of the file arguments.

---

_Comment by @carljm on 2025-05-19 16:07_

Oh, so even the "too many iterations" panic only reproduces multi-threaded?

---

_Comment by @MichaReiser on 2025-05-19 16:09_

Only if you remove some statements :) I hope that it's related to the issue we discussed in our call today

---

_Comment by @carljm on 2025-05-19 16:10_

Ok, but if you _don't_ remove some statements, there's also a single-threaded runaway iteration issue we need to understand here.

---

_Comment by @sharkdp on 2025-05-19 17:52_

> Oh, so even the "too many iterations" panic only reproduces multi-threaded?

No. The panic reproduces single threaded and multi-threaded, as expected. With the MRE from the post above.

The hang only reproduces multi-threaded, when `class Mapping(Adapter): ...` is removed. Whether or not it's related to a similar issue, I have no idea. It just seemed likely, given how small this example is.

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:27_

---

_Removed from milestone `Beta` by @carljm on 2025-08-15 15:13_

---

_Added to milestone `GA` by @carljm on 2025-08-15 15:13_

---

_Comment by @MichaReiser on 2025-10-17 09:16_

This seems to be fixed now. 

@sharkdp's repro type checks immediately.

The MRE from the issue description also completes immediately but I'm probably doing something wrong because ty fails to resolve `Float32l` (with any version, even as early as the first alpha)

---

_Comment by @MichaReiser on 2025-10-17 09:32_

ah lol... I had a `construct.py` from @sharkdp's MRE. This is why the import couldn't be resolved. The MRE from the issue author still reproduces

---

_Comment by @MichaReiser on 2025-10-27 07:00_

There's another repro in https://github.com/astral-sh/ty/issues/1437 

---

_Closed by @MichaReiser on 2025-11-13 14:38_

---
