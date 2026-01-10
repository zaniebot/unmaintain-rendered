```yaml
number: 880
title: "[panic] in ty when checking code"
type: issue
state: closed
author: ilitzroth
labels: []
assignees: []
created_at: 2025-07-24T07:52:48Z
updated_at: 2025-07-24T08:02:37Z
url: https://github.com/astral-sh/ty/issues/880
synced_at: 2026-01-10T02:06:24Z
```

# [panic] in ty when checking code

---

_Issue opened by @ilitzroth on 2025-07-24 07:52_

code typechecks with mypy.

> error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/fc00eba/src/function/fetch.rs:165:25 when checking `/arakoon-ee/pyrakoon/src/pyrakoon/protocol/messages.py`: `dependency graph cycle when querying PEP695TypeAliasType < 'db >::value_type_(Id(38401)), set cycle_fn/cycle_initial to fixpoint iterate.
> Query stack:
> [
>     check_file_impl(Id(c1f)),
>     infer_scope_types(Id(13c00)),
>     infer_definition_types(Id(14017)),
>     Type < 'db >::member_lookup_with_policy_(Id(2bc17)),
>     place_by_id(Id(2c012)),
>     infer_definition_types(Id(f9a8)),
>     infer_expression_types(Id(fd04)),
>     infer_definition_types(Id(f9a7)),
>     PEP695TypeAliasType < 'db >::value_type_(Id(38401)),
>     infer_scope_types(Id(ec55)),
>     place_by_id(Id(2c019)),
>     infer_deferred_types(Id(20359)),
>     infer_definition_types(Id(287c)),
>     infer_expression_types(Id(3824)),
>     infer_definition_types(Id(1d32)),
>     infer_expression_types(Id(3445)),
>     infer_definition_types(Id(1d39)),
>     infer_expression_types(Id(344c)),
> ]`
> info: This indicates a bug in ty.
> info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
> info: Platform: linux x86_64
> info: Version: 0.0.1-alpha.15
> info: Args: ["ty", "check", "--color=never"]
> info: Backtrace:
>    0: <unknown>
>    1: <unknown>
>    2: <unknown>
>    3: <unknown>
>    4: <unknown>
>    5: <unknown>
>    6: <unknown>
>    7: <unknown>
>    8: <unknown>
>    9: <unknown>
>   10: <unknown>
>   11: <unknown>
>   12: <unknown>
>   13: <unknown>
>   14: <unknown>
>   15: <unknown>
>   16: <unknown>
>   17: <unknown>
>   18: <unknown>
>   19: <unknown>
>   20: <unknown>
>   21: <unknown>
>   22: <unknown>
>   23: <unknown>
>   24: <unknown>
>   25: <unknown>
>   26: <unknown>
>   27: <unknown>
>   28: <unknown>
>   29: <unknown>
>   30: <unknown>
>   31: <unknown>
>   32: <unknown>
>   33: <unknown>
>   34: <unknown>
>   35: <unknown>
>   36: <unknown>
>   37: <unknown>
>   38: <unknown>
>   39: <unknown>
>   40: <unknown>
>   41: <unknown>
>   42: <unknown>
>   43: <unknown>
>   44: <unknown>
>   45: <unknown>
>   46: <unknown>
>   47: <unknown>
>   48: <unknown>
>   49: <unknown>
>   50: <unknown>
>   51: <unknown>
>   52: <unknown>
>   53: <unknown>
>   54: <unknown>
>   55: <unknown>
>   56: <unknown>
>   57: <unknown>
>   58: <unknown>
>   59: <unknown>
>   60: <unknown>
>   61: <unknown>
>   62: <unknown>
>   63: <unknown>
>   64: <unknown>
>   65: <unknown>
>   66: <unknown>
>   67: <unknown>
>   68: <unknown>
>   69: <unknown>
>   70: <unknown>
>   71: <unknown>
>   72: <unknown>
>   73: <unknown>
>   74: <unknown>
>   75: <unknown>
>   76: <unknown>
>   77: <unknown>
>   78: <unknown>
>   79: <unknown>
>   80: <unknown>
>   81: <unknown>
>   82: <unknown>
>   83: <unknown>
>   84: <unknown>
>   85: <unknown>
>   86: <unknown>
>   87: <unknown>
>   88: <unknown>
>   89: <unknown>
>   90: <unknown>
>   91: <unknown>
>   92: <unknown>
>   93: <unknown>
>   94: <unknown>
>   95: <unknown>
>   96: <unknown>
>   97: <unknown>
>   98: <unknown>
>   99: <unknown>
>  100: <unknown>
>  101: <unknown>
>  102: <unknown>
>  103: <unknown>
>  104: <unknown>
>  105: <unknown>
>  106: <unknown>
>  107: <unknown>
>  108: <unknown>
>  109: <unknown>
>  110: <unknown>
>  111: <unknown>
>  112: <unknown>
>  113: clone
> 
> info: query stacktrace:
>    0: infer_scope_types(Id(ec55))
>              at crates/ty_python_semantic/src/types/infer.rs:129
>    1: PEP695TypeAliasType < 'db >::value_type_(Id(38401))
>              at crates/ty_python_semantic/src/types.rs:7724
>    2: infer_definition_types(Id(f9a7))
>              at crates/ty_python_semantic/src/types/infer.rs:158
>    3: infer_expression_types(Id(fd04))
>              at crates/ty_python_semantic/src/types/infer.rs:234
>    4: infer_definition_types(Id(f9a8))
>              at crates/ty_python_semantic/src/types/infer.rs:158
>    5: place_by_id(Id(2c012))
>              at crates/ty_python_semantic/src/place.rs:638
>    6: Type < 'db >::member_lookup_with_policy_(Id(2bc17))
>              at crates/ty_python_semantic/src/types.rs:596
>    7: infer_definition_types(Id(14017))
>              at crates/ty_python_semantic/src/types/infer.rs:158
>    8: infer_scope_types(Id(13c00))
>              at crates/ty_python_semantic/src/types/infer.rs:129
>    9: check_file_impl(Id(c1f))
>              at crates/ty_project/src/lib.rs:482

---

_Comment by @sharkdp on 2025-07-24 08:02_

Thank you very much for reporting this.

It looks like a duplicate of #256.

---

_Closed by @sharkdp on 2025-07-24 08:02_

---
