```yaml
number: 683
title: "[panic] `infer_definition_types(Id(d67c)): execute: too many cycle iterations`"
type: issue
state: closed
author: remiconnesson
labels:
  - bug
  - fatal
assignees: []
created_at: 2025-06-19T16:23:45Z
updated_at: 2025-06-19T20:41:54Z
url: https://github.com/astral-sh/ty/issues/683
synced_at: 2026-01-10T02:08:20Z
```

# [panic] `infer_definition_types(Id(d67c)): execute: too many cycle iterations`

---

_Issue opened by @remiconnesson on 2025-06-19 16:23_

# Minimal Repro

Running this working code in a notebook 

```python
import weaviate.classes as wvc
import weaviate
client = weaviate.connect_to_local()

collection = client.collections.get("...")

def bm25_search(query, query_properties):
    return collection.query.bm25(
        query=query,
        query_properties=[*query_properties],
        limit=3,
        return_metadata=wvc.query.MetadataQuery.full(),
    )


res = bm25_search(query, query_properties)
```

with
```shell
$ uv pip list | grep weaviate
weaviate-client                          4.15.2
```

results in this panic:

```
RUST_BACKTRACE=1 ty check notebooks/04.ipynb
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/09627e4/src/function/execute.rs:212:25 when checking `/home/remi/experiments/llm/002_icpr_ar6/rag/notebooks/04.ipynb`: `infer_definition_types(Id(c960)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Args: ["ty", "check", "notebooks/04.ipynb"]
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
  95: <unknown>
  96: <unknown>
  97: <unknown>
  98: <unknown>
  99: <unknown>
 100: <unknown>
 101: <unknown>
 102: <unknown>
 103: <unknown>
 104: <unknown>
 105: <unknown>
 106: <unknown>
 107: <unknown>
 108: <unknown>
 109: <unknown>
 110: <unknown>
 111: <unknown>
 112: <unknown>
 113: <unknown>
 114: <unknown>
 115: <unknown>
 116: <unknown>
 117: <unknown>
 118: <unknown>
 119: <unknown>
 120: <unknown>
 121: <unknown>
 122: <unknown>
 123: <unknown>
 124: <unknown>
 125: <unknown>
 126: <unknown>
 127: <unknown>
 128: <unknown>
 129: <unknown>
 130: <unknown>
 131: <unknown>
 132: <unknown>
 133: <unknown>
 134: <unknown>
 135: <unknown>
 136: <unknown>
 137: <unknown>
 138: <unknown>
 139: <unknown>
 140: <unknown>
 141: <unknown>
 142: <unknown>
 143: <unknown>
 144: <unknown>
 145: <unknown>
 146: <unknown>
 147: <unknown>

info: query stacktrace:
   0: infer_expression_types(Id(b77d))
             at crates/ty_python_semantic/src/types/infer.rs:243
   1: infer_definition_types(Id(c99d))
             at crates/ty_python_semantic/src/types/infer.rs:167
   2: infer_expression_types(Id(b77f))
             at crates/ty_python_semantic/src/types/infer.rs:243
   3: infer_definition_types(Id(c99f))
             at crates/ty_python_semantic/src/types/infer.rs:167
   4: place_by_id(Id(309d))
             at crates/ty_python_semantic/src/place.rs:597
   5: member_lookup_with_policy_(Id(2d35))
             at crates/ty_python_semantic/src/types.rs:561
   6: infer_definition_types(Id(cb4c))
             at crates/ty_python_semantic/src/types/infer.rs:167
   7: infer_definition_types(Id(d02f))
             at crates/ty_python_semantic/src/types/infer.rs:167
   8: member_lookup_with_policy_(Id(2d2f))
             at crates/ty_python_semantic/src/types.rs:561
   9: member_lookup_with_policy_(Id(2d2d))
             at crates/ty_python_semantic/src/types.rs:561
  10: infer_scope_types(Id(1001))
             at crates/ty_python_semantic/src/types/infer.rs:138
  11: check_types(Id(c00))
             at crates/ty_python_semantic/src/types.rs:88


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

---

_Label `bug` added by @AlexWaygood on 2025-06-19 16:26_

---

_Label `fatal` added by @AlexWaygood on 2025-06-19 16:26_

---

_Comment by @AlexWaygood on 2025-06-19 19:49_

More minimal repro:

```py
from weaviate.collections.classes.internal import QuerySearchReturnType

x: QuerySearchReturnType
```

---

_Comment by @AlexWaygood on 2025-06-19 20:41_

And an even more minimal repro:

```py
from weaviate.collections.classes.internal import CrossReferences

x: CrossReferences
```

`CrossReferences` [is a recursive type alias](https://github.com/weaviate/weaviate-python-client/blob/96b0638517891be49f4973ae3840fcd809080466/weaviate/collections/classes/internal.py#L528), so this is #256 

---

_Closed by @AlexWaygood on 2025-06-19 20:41_

---
