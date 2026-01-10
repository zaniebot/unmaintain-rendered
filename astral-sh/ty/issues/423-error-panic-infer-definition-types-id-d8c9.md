```yaml
number: 423
title: "error[panic]: infer_definition_types(Id(d8c9)): execute: too many cycle iterations"
type: issue
state: closed
author: Alicimo
labels:
  - fatal
assignees: []
created_at: 2025-05-16T12:13:31Z
updated_at: 2025-08-15T15:14:42Z
url: https://github.com/astral-sh/ty/issues/423
synced_at: 2026-01-10T02:06:24Z
```

# error[panic]: infer_definition_types(Id(d8c9)): execute: too many cycle iterations

---

_Issue opened by @Alicimo on 2025-05-16 12:13_

### Summary


Cmd:
`RUST_BACKTRACE=1 uvx ty check tests/test_app.py `

Output:
```
error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/7edce6e/src/function/execute.rs:209:25 when checking `***`: `infer_definition_types(Id(d8c9)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Args: ["ty", "check", "tests/test_app.py"]
info: Backtrace:
   0: __mh_execute_header
   1: __mh_execute_header
   2: __mh_execute_header
   3: __mh_execute_header
   4: __mh_execute_header
   5: __mh_execute_header
   6: __mh_execute_header
   7: __mh_execute_header
   8: __mh_execute_header
   9: __mh_execute_header
  10: __mh_execute_header
  11: __mh_execute_header
  12: __mh_execute_header
  13: __mh_execute_header
  14: __mh_execute_header
  15: __mh_execute_header
  16: __mh_execute_header
  17: __mh_execute_header
  18: __mh_execute_header
  19: __mh_execute_header
  20: __mh_execute_header
  21: __mh_execute_header
  22: __mh_execute_header
  23: __mh_execute_header
  24: __mh_execute_header
  25: __mh_execute_header
  26: __mh_execute_header
  27: __mh_execute_header
  28: __mh_execute_header
  29: __mh_execute_header
  30: __mh_execute_header
  31: __mh_execute_header
  32: __mh_execute_header
  33: __mh_execute_header
  34: __mh_execute_header
  35: __mh_execute_header
  36: __mh_execute_header
  37: __mh_execute_header
  38: __mh_execute_header
  39: __mh_execute_header
  40: __mh_execute_header
  41: __mh_execute_header
  42: __mh_execute_header
  43: __mh_execute_header
  44: __mh_execute_header
  45: __mh_execute_header
  46: __mh_execute_header
  47: __mh_execute_header
  48: __mh_execute_header
  49: __mh_execute_header
  50: __mh_execute_header
  51: __mh_execute_header
  52: __mh_execute_header
  53: __mh_execute_header
  54: __mh_execute_header
  55: __mh_execute_header
  56: __mh_execute_header
  57: __mh_execute_header
  58: __mh_execute_header
  59: __mh_execute_header
  60: __mh_execute_header
  61: __mh_execute_header
  62: __mh_execute_header
  63: __mh_execute_header
  64: __mh_execute_header
  65: __mh_execute_header
  66: __mh_execute_header
  67: __mh_execute_header
  68: __mh_execute_header
  69: __mh_execute_header
  70: __mh_execute_header
  71: __mh_execute_header
  72: __mh_execute_header
  73: __mh_execute_header
  74: __mh_execute_header
  75: __mh_execute_header
  76: __mh_execute_header
  77: __mh_execute_header
  78: __mh_execute_header
  79: __mh_execute_header
  80: __mh_execute_header
  81: __mh_execute_header
  82: __mh_execute_header
  83: __mh_execute_header
  84: __mh_execute_header
  85: __mh_execute_header
  86: __mh_execute_header
  87: __mh_execute_header
  88: __mh_execute_header
  89: __mh_execute_header
  90: __mh_execute_header
  91: __mh_execute_header
  92: __mh_execute_header
  93: __mh_execute_header
  94: __mh_execute_header
  95: __mh_execute_header
  96: __mh_execute_header
  97: __mh_execute_header
  98: __mh_execute_header
  99: __mh_execute_header
 100: __mh_execute_header
 101: __mh_execute_header
 102: __mh_execute_header
 103: __mh_execute_header
 104: __mh_execute_header
 105: __mh_execute_header
 106: __mh_execute_header
 107: __mh_execute_header
 108: __mh_execute_header
 109: __mh_execute_header
 110: __mh_execute_header
 111: __mh_execute_header
 112: __mh_execute_header
 113: __pthread_cond_wait

info: query stacktrace:
   0: symbol_by_id(Id(3152))
             at crates/ty_python_semantic/src/symbol.rs:577
   1: member_lookup_with_policy_(Id(2d8d))
             at crates/ty_python_semantic/src/types.rs:536
   2: infer_definition_types(Id(142b))
             at crates/ty_python_semantic/src/types/infer.rs:149
   3: symbol_by_id(Id(3151))
             at crates/ty_python_semantic/src/symbol.rs:577
   4: infer_deferred_types(Id(143a))
             at crates/ty_python_semantic/src/types/infer.rs:187
   5: signature_(Id(4026))
             at crates/ty_python_semantic/src/types.rs:6606
   6: infer_expression_types(Id(1800))
             at crates/ty_python_semantic/src/types/infer.rs:223
   7: infer_definition_types(Id(1403))
             at crates/ty_python_semantic/src/types/infer.rs:149
   8: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:122
   9: check_types(Id(c00))
             at crates/ty_python_semantic/src/types.rs:84


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

### Version

`ty 0.0.1-alpha.3 (144a26d44 2025-05-15)`

---

_Comment by @carljm on 2025-05-16 12:17_

Thanks for the report! We do have some known causes for this bug already, so we will work on those, and once we've fixed the issues we know about, you can test again and see if you still see the problem. (Though if the code on which this occurs is open source, feel free to point us to it and we can test ourselves.)

---

_Comment by @Alicimo on 2025-05-16 12:24_

Sadly I can't share the full codebase. I'm happy to share the specific file though

```
import pytest
from litestar.testing import TestClient

from src.app import app

client = TestClient(app)


@pytest.fixture
def json_input() -> dict:
    return {}


def test_predict_valid_input(json_input: dict):
    """Test API with complete valid input"""
    response = client.post("/predict", json=json_input)
    assert response.status_code == 201
    assert "risk_score" in response.json()
    assert "risk_class" in response.json()


def test_predict_invalid_input():
    """Test API with completely invalid input"""
    response = client.post("/predict", json={"invalid": "data"})
    assert response.status_code == 400  # Validation error
```


---

_Comment by @danielhollas on 2025-05-16 12:43_

> I'm happy to share the specific file though

Unfortunately typechecking this file alone does not reproduce the panic, I just get the `unresolved-import` diagnostics

```
error[unresolved-import]: Cannot resolve imported module `litestar.testing`
 --> o.py:2:6
  |
1 | import pytest
2 | from litestar.testing import TestClient
  |      ^^^^^^^^^^^^^^^^
3 |
4 | from src.app import app
  |
info: rule `unresolved-import` is enabled by default

error[unresolved-import]: Cannot resolve imported module `src.app`
 --> o.py:4:6
  |
2 | from litestar.testing import TestClient
3 |
4 | from src.app import app
  |      ^^^^^^^
5 |
6 | client = TestClient(app)
  |
info: rule `unresolved-import` is enabled by default

Found 2 diagnostics
```

You can try commenting out one or the other import to see where the panic comes from.

---

_Comment by @MichaReiser on 2025-05-16 12:48_

Can you try running with `TY_MAX_PARALLELISM=1 ty check -vvv`. This will produce a ton of output. Can you share the last few log messages. They might give us an idea which file ty is checking right before hitting too many iterations

---

_Label `fatal` added by @AlexWaygood on 2025-05-16 12:53_

---

_Label `needs-mre` added by @AlexWaygood on 2025-05-16 12:53_

---

_Comment by @Alicimo on 2025-05-21 07:18_

> You can try commenting out one or the other import to see where the panic comes from.

Doing this suggests `litestar.testing import TestClient` is the culprit. The errors change to ```error[unresolved-reference]: Name `TestClient` used when not defined```, which is expected given the now missing import. Changing that line to `client = app` gives further expected errors.

Output of `TY_MAX_PARALLELISM=1 uvx ty check tests/test_app.py`

```
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Args: ["ty", "check", "tests/test_app.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: symbol_by_id(Id(3152))
             at crates/ty_python_semantic/src/symbol.rs:579
   1: member_lookup_with_policy_(Id(2d8d))
             at crates/ty_python_semantic/src/types.rs:550
   2: infer_definition_types(Id(142b))
             at crates/ty_python_semantic/src/types/infer.rs:148
   3: symbol_by_id(Id(3151))
             at crates/ty_python_semantic/src/symbol.rs:579
   4: infer_deferred_types(Id(143a))
             at crates/ty_python_semantic/src/types/infer.rs:186
   5: signature_(Id(4026))
             at crates/ty_python_semantic/src/types.rs:6845
   6: infer_expression_types(Id(1800))
             at crates/ty_python_semantic/src/types/infer.rs:222
   7: infer_definition_types(Id(1403))
             at crates/ty_python_semantic/src/types/infer.rs:148
   8: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:121
   9: check_types(Id(c00))
             at crates/ty_python_semantic/src/types.rs:84
```



---

_Comment by @qarmin on 2025-06-13 21:11_

Python file
```
from packaging.requirements import Requirement

def get_requires_for_build_wheel():
    Requirement("").marker
```
pyproject.toml
```
[project]
name = "q"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
"packaging",
]

```
panic
```
error[panic]: Panicked at /home/rafal/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/09627e4/src/function/execute.rs:212:25 when checking `/home/rafal/Downloads/FILES_3/60.py`: `infer_definition_types(Id(6abd)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Args: ["ty", "check", "."]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: place_by_id(Id(301a))
             at crates/ty_python_semantic/src/place.rs:597
   1: infer_definition_types(Id(6ac2))
             at crates/ty_python_semantic/src/types/infer.rs:164
   2: place_by_id(Id(3019))
             at crates/ty_python_semantic/src/place.rs:597
   3: class_member_with_policy_(Id(5c06))
             at crates/ty_python_semantic/src/types.rs:559
   4: infer_expression_types(Id(2c08))
             at crates/ty_python_semantic/src/types/infer.rs:240
   5: infer_expression_type(Id(2c08))
             at crates/ty_python_semantic/src/types/infer.rs:302
   6: member_lookup_with_policy_(Id(280a))
             at crates/ty_python_semantic/src/types.rs:559
   7: infer_scope_types(Id(1001))
             at crates/ty_python_semantic/src/types/infer.rs:135
   8: check_types(Id(c00))
             at crates/ty_python_semantic/src/types.rs:88

```

---

_Comment by @AlexWaygood on 2025-06-13 21:23_

The "too many iterations" panic on `packaging` is likely https://github.com/astral-sh/ty/issues/256 (due to the type aliases at https://github.com/pypa/packaging/blob/d0d5ad8687f666bea942d1ab4ee2feb5fa019d04/src/packaging/_parser.py#L44-L47)

---

_Added to milestone `Beta` by @MichaReiser on 2025-08-15 12:14_

---

_Label `needs-mre` removed by @MichaReiser on 2025-08-15 12:14_

---

_Closed by @carljm on 2025-08-15 15:14_

---
