```yaml
number: 1524
title: "[panic] infer_definition_types(Id(b8df)): execute: too many cycle iterations"
type: issue
state: closed
author: peterHoburg
labels:
  - fatal
assignees: []
created_at: 2025-11-11T19:08:10Z
updated_at: 2025-11-29T18:31:24Z
url: https://github.com/astral-sh/ty/issues/1524
synced_at: 2026-01-12T15:54:25Z
```

# [panic] infer_definition_types(Id(b8df)): execute: too many cycle iterations

---

_@peterHoburg_

Error:

```
%RUST_BACKTRACE=1 uv run ty check
error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/05a9af7/src/function/execute.rs:451:17 when checking `ty_example/main.py`: `infer_definition_types(Id(b8df)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: 0.0.1-alpha.26 (b225fd8b4 2025-11-10)
info: Args: ["ty", "check"]
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
  72: __pthread_cond_wait

info: query stacktrace:
   0: place_by_id(Id(3080))
             at crates/ty_python_semantic/src/place.rs:709
   1: Type < 'db >::member_lookup_with_policy_(Id(2c72))
             at crates/ty_python_semantic/src/types.rs:788
   2: infer_definition_types(Id(92f0))
             at crates/ty_python_semantic/src/types/infer.rs:94
   3: place_by_id(Id(307f))
             at crates/ty_python_semantic/src/place.rs:709
   4: infer_definition_types(Id(934e))
             at crates/ty_python_semantic/src/types/infer.rs:94
   5: Type < 'db >::member_lookup_with_policy_(Id(2c6e))
             at crates/ty_python_semantic/src/types.rs:788
   6: infer_expression_types_impl(Id(1800))
             at crates/ty_python_semantic/src/types/infer.rs:182
   7: infer_scope_types(Id(1001))
             at crates/ty_python_semantic/src/types/infer.rs:70
   8: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:535


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

I have attached a project that has the minimum replication steps. 

Here is the code that causes the issue

```py
from supabase import AsyncClient

async def example(supabase_client: AsyncClient):
    api_key_response = await (
        supabase_client.from_("")
        .select("*")
        .execute()
    )
    if len(api_key_response.data) == 0:
       return
```

The rest of the project is the uv setup to replicate it. 

[ty_example.zip](https://github.com/user-attachments/files/23484980/ty_example.zip)



---

_Label `fatal` added by @AlexWaygood on 2025-11-11 19:11_

---

_Added to milestone `Beta` by @carljm on 2025-11-13 07:33_

---

_Assigned to @carljm by @carljm on 2025-11-13 22:24_

---

_Comment by @mtshiba on 2025-11-14 15:11_

The cause of this is likely #256, and the code provided works without panicking in astral-sh/ruff#20566, which addresses the issue.

---

_Closed by @carljm on 2025-11-14 16:27_

---

_Comment by @carljm on 2025-11-14 16:28_

Thank you! Testing whether https://github.com/astral-sh/ruff/pull/20566 fixes this was indeed the action item I had in mind when I assigned this to myself, but hadn't gotten to it yet :)

---

_Comment by @carljm on 2025-11-14 16:29_

And thanks @peterHoburg for the excellent report with clear repro!

---

_Comment by @peterHoburg on 2025-11-14 20:26_

Thank you for looking into it! 

---

_Closed by @carljm on 2025-11-26 16:50_

---

_Comment by @peterHoburg on 2025-11-29 18:31_

Hey. I just wanted to follow up on this. The issue is fixed on 29! Thanks all! 

---
