```yaml
number: 854
title: "[panic]  not sure of cause"
type: issue
state: closed
author: justinvanwinkle
labels: []
assignees: []
created_at: 2025-07-18T18:32:11Z
updated_at: 2025-07-21T12:37:59Z
url: https://github.com/astral-sh/ty/issues/854
synced_at: 2026-01-10T02:07:36Z
```

# [panic]  not sure of cause

---

_Issue opened by @justinvanwinkle on 2025-07-18 18:32_

```
error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/fc00eba/src/function/fetch.rs:165:25 when checking `/home/jvanwinkle/pk/parkade-server/park/api/data_source_test_base.py`: `dependency graph cycle when querying PEP695TypeAliasType < 'db >::value_type_(Id(196401)), set cycle_fn/cycle_initial to fixpoint iterate.
Query stack:
[
    check_file_impl(Id(c4c)),
    infer_scope_types(Id(3f80a)),
    infer_definition_types(Id(4045d)),
    infer_expression_types(Id(44418)),
    FunctionType < 'db >::signature_(Id(10fc00)),
    FunctionLiteral < 'db >::overloads_and_implementation_(Id(10f400)),
    infer_definition_types(Id(30c2)),
    PEP695TypeAliasType < 'db >::value_type_(Id(196401)),
    infer_scope_types(Id(2810)),
]`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.15
info: Args: ["ty", "check", "park/api"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_scope_types(Id(2810))
             at crates/ty_python_semantic/src/types/infer.rs:129
   1: PEP695TypeAliasType < 'db >::value_type_(Id(196401))
             at crates/ty_python_semantic/src/types.rs:7724
   2: infer_definition_types(Id(30c2))
             at crates/ty_python_semantic/src/types/infer.rs:158
   3: FunctionLiteral < 'db >::overloads_and_implementation_(Id(10f400))
             at crates/ty_python_semantic/src/types/function.rs:436
   4: FunctionType < 'db >::signature_(Id(10fc00))
             at crates/ty_python_semantic/src/types/function.rs:596
   5: infer_expression_types(Id(44418))
             at crates/ty_python_semantic/src/types/infer.rs:234
   6: infer_definition_types(Id(4045d))
             at crates/ty_python_semantic/src/types/infer.rs:158
   7: infer_scope_types(Id(3f80a))
             at crates/ty_python_semantic/src/types/infer.rs:129
   8: check_file_impl(Id(c4c))
             at crates/ty_project/src/lib.rs:482


error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/fc00eba/src/function/fetch.rs:165:25 when checking `/home/jvanwinkle/pk/parkade-server/park/api/lot_test.py`: `dependency graph cycle when querying PEP695TypeAliasType < 'db >::value_type_(Id(196401)), set cycle_fn/cycle_initial to fixpoint iterate.
Query stack:
[
    check_file_impl(Id(c3a)),
    infer_scope_types(Id(5ac2a)),
    FunctionType < 'db >::signature_(Id(10fc00)),
    FunctionLiteral < 'db >::overloads_and_implementation_(Id(10f400)),
    infer_definition_types(Id(30c2)),
    PEP695TypeAliasType < 'db >::value_type_(Id(196401)),
    infer_scope_types(Id(2810)),
    parsed_module(Id(efc2d)),
    source_text(Id(efc2d)),
]`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.15
info: Args: ["ty", "check", "park/api"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_scope_types(Id(2810))
             at crates/ty_python_semantic/src/types/infer.rs:129
   1: PEP695TypeAliasType < 'db >::value_type_(Id(196401))
             at crates/ty_python_semantic/src/types.rs:7724
   2: infer_definition_types(Id(30c2))
             at crates/ty_python_semantic/src/types/infer.rs:158
   3: FunctionLiteral < 'db >::overloads_and_implementation_(Id(10f400))
             at crates/ty_python_semantic/src/types/function.rs:436
   4: FunctionType < 'db >::signature_(Id(10fc00))
             at crates/ty_python_semantic/src/types/function.rs:596
   5: infer_scope_types(Id(5ac2a))
             at crates/ty_python_semantic/src/types/infer.rs:129
   6: check_file_impl(Id(c3a))
             at crates/ty_project/src/lib.rs:482
```

---

_Comment by @carljm on 2025-07-18 18:44_

Looking at the query stack, I'm pretty sure this is some kind of recursive PEP 695 type alias, so a duplicate of #256. Thanks for the report!

---

_Closed by @carljm on 2025-07-18 18:44_

---

_Comment by @MichaReiser on 2025-07-21 12:36_

This can be reproduced in homeassistant-core. After cloning, setup the venv with `script/setup`

---

_Comment by @sharkdp on 2025-07-21 12:37_

> This can be reproduced in homeassistant-core. After cloning, setup the venv with `script/setup`

Yes, see https://github.com/astral-sh/ruff/blob/b6579eaf04cd2031a57ec7558326a7a8bd26d410/crates/ty_python_semantic/resources/primer/bad.txt#L5

---
