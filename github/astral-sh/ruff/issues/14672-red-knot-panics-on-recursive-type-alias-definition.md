---
number: 14672
title: "[red-knot] Panics on recursive type alias definition"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - ty
assignees: []
created_at: 2024-11-29T08:13:53Z
updated_at: 2025-03-12T12:41:43Z
url: https://github.com/astral-sh/ruff/issues/14672
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Panics on recursive type alias definition

---

_Issue opened by @dhruvmanila on 2024-11-29 08:13_

```python
type Expr = tuple[int, Expr]
```

<details><summary>Panic traceback:</summary>
<p>

```console
$ RUST_BACKTRACE=1 cargo run --bin red_knot -- --current-directory=~/playground/ruff/type_inference/fuzzer -vv
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.12s
     Running `target/debug/red_knot --current-directory=/Users/dhruv/playground/ruff/type_inference/fuzzer -vv`
2024-11-29 13:39:13.114098000 DEBUG Searching for a workspace in '/Users/dhruv/playground/ruff/type_inference/fuzzer'
2024-11-29 13:39:13.115415000 DEBUG Single package workspace at '/Users/dhruv/playground/ruff/type_inference/fuzzer'
2024-11-29 13:39:13.115956000 INFO Target version: Python 3.9
2024-11-29 13:39:13.116077000 DEBUG Adding first-party search path '/Users/dhruv/playground/ruff/type_inference/fuzzer'
2024-11-29 13:39:13.116169000 DEBUG Using vendored stdlib
2024-11-29 13:39:13.119593000 DEBUG Starting main loop
2024-11-29 13:39:13.119844000 DEBUG Waiting for next main loop message.
2024-11-29 13:39:13.120015000 DEBUG Checking workspace
2024-11-29 13:39:13.124805000 INFO Found 1 files in package `fuzzer`
2024-11-29 13:39:13.125295000 DEBUG Checking file '/Users/dhruv/playground/ruff/type_inference/fuzzer/main.py'
thread '<unnamed>' panicked at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/runtime.rs:341:13:
Box<dyn Any>
stack backtrace:
   0: std::panicking::begin_panic
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panicking.rs:734:5
   1: std::panic::panic_any
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panic.rs:259:5
   2: salsa::runtime::Runtime::unblock_cycle_and_maybe_throw
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/runtime.rs:341:13
   3: salsa::runtime::Runtime::block_on_or_unwind
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/runtime.rs:191:13
   4: salsa::zalsa::Zalsa::block_on_or_unwind
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/zalsa.rs:277:9
   5: salsa::table::sync::SyncTable::claim
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/table/sync.rs:71:17
   6: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:64:28
   7: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:36:67
   8: core::option::Option<T>::or_else
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/option.rs:1545:21
   9: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:36:33
  10: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:13:20
  11: red_knot_python_semantic::types::infer::infer_scope_types::{{closure}}
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/components/salsa-macro-rules/src/setup_tracked_fn.rs:278:25
  12: salsa::attach::Attached::attach
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/attach.rs:71:9
  13: salsa::attach::attach::{{closure}}
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/attach.rs:91:23
  14: std::thread::local::LocalKey<T>::try_with
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/thread/local.rs:283:12
  15: std::thread::local::LocalKey<T>::with
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/thread/local.rs:260:9
  16: salsa::attach::attach
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/attach.rs:91:5
  17: red_knot_python_semantic::types::infer::infer_scope_types
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/components/salsa-macro-rules/src/setup_tracked_fn.rs:270:13
  18: red_knot_python_semantic::types::definition_expression_ty
             at ./crates/red_knot_python_semantic/src/types.rs:221:9
  19: <red_knot_python_semantic::types::TypeAliasType as red_knot_python_semantic::types::TypeAliasType::value_ty::InnerTrait_>::inner_fn_name_
             at ./crates/red_knot_python_semantic/src/types.rs:2829:9
  20: <red_knot_python_semantic::types::TypeAliasType::value_ty::inner_fn_name_::Configuration_ as salsa::function::Configuration>::execute::inner_
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/components/salsa-macro-rules/src/setup_method_body.rs:34:17
  21: <red_knot_python_semantic::types::TypeAliasType::value_ty::inner_fn_name_::Configuration_ as salsa::function::Configuration>::execute
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/components/salsa-macro-rules/src/setup_tracked_fn.rs:179:21
  22: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute::{{closure}}
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/execute.rs:51:43
  23: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/panic/unwind_safe.rs:272:9
  24: std::panicking::try::do_call
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panicking.rs:554:40
  25: ___rust_try
  26: std::panicking::try
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panicking.rs:518:19
  27: std::panic::catch_unwind
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panic.rs:345:14
  28: salsa::cycle::Cycle::catch
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/cycle.rs:42:15
  29: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/execute.rs:51:27
  30: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:86:14
  31: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:36:67
  32: core::option::Option<T>::or_else
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/option.rs:1545:21
  33: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:36:33
  34: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:13:20
  35: red_knot_python_semantic::types::TypeAliasType::value_ty::inner_fn_name_::{{closure}}
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/components/salsa-macro-rules/src/setup_tracked_fn.rs:278:25
  36: salsa::attach::Attached::attach
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/attach.rs:71:9
  37: salsa::attach::attach::{{closure}}
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/attach.rs:91:23
  38: std::thread::local::LocalKey<T>::try_with
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/thread/local.rs:283:12
  39: std::thread::local::LocalKey<T>::with
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/thread/local.rs:260:9
  40: salsa::attach::attach
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/attach.rs:91:5
  41: red_knot_python_semantic::types::TypeAliasType::value_ty::inner_fn_name_
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/components/salsa-macro-rules/src/setup_tracked_fn.rs:270:13
  42: red_knot_python_semantic::types::TypeAliasType::value_ty
             at ./crates/red_knot_python_semantic/src/types.rs:2820:1
  43: red_knot_python_semantic::types::Type::in_type_expression
             at ./crates/red_knot_python_semantic/src/types.rs:1555:77
  44: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_type_expression_no_store
             at ./crates/red_knot_python_semantic/src/types/infer.rs:4304:21
  45: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_type_expression
             at ./crates/red_knot_python_semantic/src/types/infer.rs:4268:18
  46: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_tuple_type_expression
             at ./crates/red_knot_python_semantic/src/types/infer.rs:4517:38
  47: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_type_expression_no_store
             at ./crates/red_knot_python_semantic/src/types/infer.rs:4347:56
  48: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_annotation_expression_impl
             at ./crates/red_knot_python_semantic/src/types/infer.rs:4238:26
  49: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_annotation_expression
             at ./crates/red_knot_python_semantic/src/types/infer.rs:4188:29
  50: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_type_alias
             at ./crates/red_knot_python_semantic/src/types/infer.rs:870:9
  51: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region_scope
             at ./crates/red_knot_python_semantic/src/types/infer.rs:446:17
  52: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region
             at ./crates/red_knot_python_semantic/src/types/infer.rs:419:46
  53: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
             at ./crates/red_knot_python_semantic/src/types/infer.rs:4172:9
  54: <red_knot_python_semantic::types::infer::infer_scope_types::Configuration_ as salsa::function::Configuration>::execute::inner_
             at ./crates/red_knot_python_semantic/src/types/infer.rs:82:5
  55: <red_knot_python_semantic::types::infer::infer_scope_types::Configuration_ as salsa::function::Configuration>::execute
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/components/salsa-macro-rules/src/setup_tracked_fn.rs:179:21
  56: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute::{{closure}}
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/execute.rs:51:43
  57: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/panic/unwind_safe.rs:272:9
  58: std::panicking::try::do_call
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panicking.rs:554:40
  59: ___rust_try
  60: std::panicking::try
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panicking.rs:518:19
  61: std::panic::catch_unwind
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panic.rs:345:14
  62: salsa::cycle::Cycle::catch
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/cycle.rs:42:15
  63: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/execute.rs:51:27
  64: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:86:14
  65: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:36:67
  66: core::option::Option<T>::or_else
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/option.rs:1545:21
  67: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:36:33
  68: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:13:20
  69: red_knot_python_semantic::types::infer::infer_scope_types::{{closure}}
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/components/salsa-macro-rules/src/setup_tracked_fn.rs:278:25
  70: salsa::attach::Attached::attach
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/attach.rs:71:9
  71: salsa::attach::attach::{{closure}}
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/attach.rs:91:23
  72: std::thread::local::LocalKey<T>::try_with
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/thread/local.rs:283:12
  73: std::thread::local::LocalKey<T>::with
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/thread/local.rs:260:9
  74: salsa::attach::attach
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/attach.rs:91:5
  75: red_knot_python_semantic::types::infer::infer_scope_types
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/components/salsa-macro-rules/src/setup_tracked_fn.rs:270:13
  76: <red_knot_python_semantic::types::check_types::Configuration_ as salsa::function::Configuration>::execute::inner_
             at ./crates/red_knot_python_semantic/src/types.rs:53:22
  77: <red_knot_python_semantic::types::check_types::Configuration_ as salsa::function::Configuration>::execute
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/components/salsa-macro-rules/src/setup_tracked_fn.rs:179:21
  78: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute::{{closure}}
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/execute.rs:51:43
  79: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/panic/unwind_safe.rs:272:9
  80: std::panicking::try::do_call
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panicking.rs:554:40
  81: ___rust_try
  82: std::panicking::try
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panicking.rs:518:19
  83: std::panic::catch_unwind
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panic.rs:345:14
  84: salsa::cycle::Cycle::catch
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/cycle.rs:42:15
  85: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/execute.rs:51:27
  86: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:86:14
  87: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:36:67
  88: core::option::Option<T>::or_else
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/option.rs:1545:21
  89: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:36:33
  90: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:13:20
  91: red_knot_python_semantic::types::check_types::{{closure}}
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/components/salsa-macro-rules/src/setup_tracked_fn.rs:278:25
  92: salsa::attach::Attached::attach
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/attach.rs:71:9
  93: salsa::attach::attach::{{closure}}
             at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/attach.rs:91:23
  94: std::thread::local::LocalKey<T>::try_with
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/thread/local.rs:283:12
  95: std::thread::local::LocalKey<T>::with
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/thread/local.rs:260:9
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
Rayon: detected unexpected panic; aborting
zsh: abort      RUST_BACKTRACE=1 cargo run --bin red_knot --  -vv
```

</p>
</details> 

---

_Label `bug` added by @dhruvmanila on 2024-11-29 08:13_

---

_Label `red-knot` added by @dhruvmanila on 2024-11-29 08:13_

---

_Comment by @dhruvmanila on 2024-11-29 10:14_

This also produces the same panic:
```py
type x = x := 1
```


---

_Comment by @sharkdp on 2024-11-29 13:08_

See also: https://github.com/astral-sh/ruff/blob/b63c2e126b928fab4180eda5d9132552beeee7fe/crates/red_knot_workspace/tests/check.rs#L273C1-L274

---

_Referenced in [astral-sh/ruff#13778](../../astral-sh/ruff/issues/13778.md) on 2024-12-02 12:40_

---

_Referenced in [astral-sh/ruff#14736](../../astral-sh/ruff/pulls/14736.md) on 2024-12-02 19:10_

---

_Referenced in [astral-sh/ty#228](../../astral-sh/ty/issues/228.md) on 2024-12-02 19:26_

---

_Referenced in [astral-sh/ruff#15161](../../astral-sh/ruff/pulls/15161.md) on 2024-12-27 19:59_

---

_Added to milestone `Red Knot Q1 2025` by @carljm on 2025-01-09 17:52_

---

_Assigned to @carljm by @carljm on 2025-03-01 17:26_

---

_Referenced in [astral-sh/ruff#14029](../../astral-sh/ruff/pulls/14029.md) on 2025-03-12 05:49_

---

_Closed by @carljm on 2025-03-12 12:41_

---

_Closed by @carljm on 2025-03-12 12:41_

---

_Referenced in [astral-sh/ty#425](../../astral-sh/ty/issues/425.md) on 2025-05-16 15:01_

---
