```yaml
number: 777
title: "Pyspark ML Vector Asembler: too many cycle iterations"
type: issue
state: closed
author: michaelneely
labels:
  - bug
  - fatal
assignees: []
created_at: 2025-07-08T09:13:48Z
updated_at: 2025-07-08T11:43:39Z
url: https://github.com/astral-sh/ty/issues/777
synced_at: 2026-01-12T15:54:24Z
```

# Pyspark ML Vector Asembler: too many cycle iterations

---

_@michaelneely_

### Summary

Instantiating a `pyspark.ml.feature.VectorAssembler` causes a cycle panic.

Reproducible Example: 
```python
from pyspark.ml.feature import VectorAssembler

VectorAssembler(
    inputCols=["col1", "col2"],
    outputCol="features",
)
```

Stack Trace:
```
error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/fc00eba/src/function/execute.rs:212:25 when checking `/workspace/test.py`: `infer_definition_types(Id(6fd5)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Args: ["ty", "check", "test.py"]
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
  84: <unknown>
  85: <unknown>
  86: <unknown>
  87: <unknown>
  88: <unknown>
  89: <unknown>
  90: <unknown>
  91: <unknown>
  92: <unknown>
  93: <unknown>
  94: <unknown>
  95: start_thread
  96: __clone

info: query stacktrace:
   0: infer_definition_types(Id(6fd8))
             at crates/ty_python_semantic/src/types/infer.rs:159
   1: place_by_id(Id(3820))
             at crates/ty_python_semantic/src/place.rs:623
   2: Type < 'db >::member_lookup_with_policy_(Id(281d))
             at crates/ty_python_semantic/src/types.rs:583
   3: infer_definition_types(Id(1425))
             at crates/ty_python_semantic/src/types/infer.rs:159
   4: infer_deferred_types(Id(31c5))
             at crates/ty_python_semantic/src/types/infer.rs:198
   5: ClassLiteral < 'db >::explicit_bases_(Id(4411))
             at crates/ty_python_semantic/src/types/class.rs:788
   6: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:130
   7: check_types(Id(c00))
             at crates/ty_python_semantic/src/types.rs:89
```
Python version: Python 3.11.6

### Version

ty 0.0.1-alpha.13

---

_Label `bug` added by @sharkdp on 2025-07-08 11:42_

---

_Label `fatal` added by @sharkdp on 2025-07-08 11:42_

---

_Comment by @sharkdp on 2025-07-08 11:43_

Thank you for reporting this. [`VectorAssembler`](https://spark.apache.org/docs/latest/api/python/_modules/pyspark/ml/feature.html#VectorAssembler) recursively inherits from a generic class that takes `VectorAssembler` itself as a paramter (CRTP-style), so this looks like a duplicate of #256  

---

_Closed by @sharkdp on 2025-07-08 11:43_

---
