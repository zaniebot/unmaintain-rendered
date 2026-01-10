```yaml
number: 652
title: "Panic `index out of bounds: the len is 73 but the index is 2499`"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fatal
assignees: []
created_at: 2025-06-13T20:22:34Z
updated_at: 2025-06-14T05:02:54Z
url: https://github.com/astral-sh/ty/issues/652
synced_at: 2026-01-10T02:08:20Z
```

# Panic `index out of bounds: the len is 73 but the index is 2499`

---

_Issue opened by @qarmin on 2025-06-13 20:22_

### Summary

With pytest installed and this file
```
from _pytest.python import is_generator, getfixturemarker, Generator
import pytest

class ReportPortal:
    @pytest.mark.trylast
    def pytest_pycollect_makeitem(self, __multicall__, collector, name, obj):
        if inspect.isclass(obj):
            pass
        elif collector.funcnamefilter(name) and hasattr(obj, '__call__') and \
                        getfixturemarker(obj) is None:
            return Generator(name, parent=collector)
```
ty crashes with message
```
error[panic]: Panicked at crates/ruff_db/src/parsed.rs:193:13 when checking `/home/rafal/Downloads/FILES_4/87.py`: `index out of bounds: the len is 73 but the index is 2499`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Args: ["ty", "check", "."]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_scope_types(Id(1002))
             at crates/ty_python_semantic/src/types/infer.rs:135
   1: check_types(Id(c00))
             at crates/ty_python_semantic/src/types.rs:88

```


Steps to reproduce
- unpack - [FILES_4.zip](https://github.com/user-attachments/files/20732502/FILES_4.zip)
- run `uv sync`
- run `ty check .`

### Version

0.0.1-alpha.10

---

_Label `bug` added by @AlexWaygood on 2025-06-13 20:26_

---

_Label `fatal` added by @AlexWaygood on 2025-06-13 20:26_

---

_Comment by @carljm on 2025-06-13 20:27_

In a debug build, we hit an assertion error:

```
error[panic]: Panicked at crates/ty_python_semantic/src/ast_node_ref.rs:80:9 when checking `/Users/carlmeyer/projects/ruff-examples/issue652/main.py`: `assertion `left == right` failed
  left: 0x6000027e4cf0
 right: 0x6000027d6ad0`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Args: ["target/debug/ty", "check", "--python-version", "3.13", "--project", "../ruff-examples/issue652"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_scope_types(Id(1002))
             at crates/ty_python_semantic/src/types/infer.rs:135
   1: check_types(Id(c00))
             at crates/ty_python_semantic/src/types.rs:88
```

---

_Comment by @carljm on 2025-06-13 20:31_

Bisects to https://github.com/astral-sh/ruff/pull/18445 -- @ibraheemdev can you take a look?

---

_Closed by @MichaReiser on 2025-06-14 05:02_

---
