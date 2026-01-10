```yaml
number: 17732
title: "[red-knot] Include salsa backtrace in check and mdtest panic messages"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/include-salsa-backtrace
created_at: 2025-04-30T07:28:35Z
updated_at: 2025-04-30T08:30:12Z
url: https://github.com/astral-sh/ruff/pull/17732
synced_at: 2026-01-10T19:03:00Z
```

# [red-knot] Include salsa backtrace in check and mdtest panic messages

---

_Pull request opened by @MichaReiser on 2025-04-30 07:28_

## Summary

Include the salsa backtrace in mdtest panics and check file panics. 

This PR also changes our panic hook from `Backtrace::force_capture` to `::capture`. We already had code in the mdtest runner to *hide* the backgrace if `RUST_BACKTRACE=1` wasn't set. I also think that it's okay to ask users to re-run the program to capture a backtrace (which isn't very useful in release builds anyway).


Closes https://github.com/astral-sh/ruff/issues/17716

## Test plan

**check_files** with `RUST_BACKTRACE=1`

```
error: panic: Panicked at crates/red_knot_python_semantic/src/types/infer.rs:3705:14 when checking `__init__.py`: `expression should belong to this TypeInference region and TypeInferenceBuilder should have inferred a type for it`
info: This indicates a bug in Red Knot.
info: If you could open an issue at https://github.com/astral-sh/ruff/issues/new?title=%5Bred-knot%5D:%20panic we'd be very appreciative!
info: Platform: macos aarch64
info: Args: ["..."]
info: Backtrace:
   0: std::backtrace::Backtrace::create
   1: ruff_db::panic::install_hook::{{closure}}::{{closure}}
   2: std::panicking::rust_panic_with_hook
   3: std::panicking::begin_panic_handler::{{closure}}
   4: std::sys::backtrace::__rust_end_short_backtrace
   5: _rust_begin_unwind
   6: core::panicking::panic_fmt
   7: core::option::expect_failed
   8: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_standalone_expression
   9: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region_definition
  10: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
  11: <red_knot_python_semantic::types::infer::infer_definition_types::Configuration_ as salsa::function::Configuration>::execute
  12: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  13: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  14: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  15: std::thread::local::LocalKey<T>::with
  16: red_knot_python_semantic::symbol::symbol_from_bindings_impl::{{closure}}
  17: red_knot_python_semantic::symbol::symbol_from_bindings_impl
  18: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_name_load
  19: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
  20: core::ops::function::impls::<impl core::ops::function::FnMut<A> for &mut F>::call_mut
  21: <alloc::collections::vec_deque::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold
  22: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter::SpecFromIter<T,I>>::from_iter
  23: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_call_expression
  24: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
  25: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region
  26: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
  27: <red_knot_python_semantic::types::infer::infer_expression_types::Configuration_ as salsa::function::Configuration>::execute
  28: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  29: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  30: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  31: std::thread::local::LocalKey<T>::with
  32: <red_knot_python_semantic::types::infer::infer_expression_type::Configuration_ as salsa::function::Configuration>::execute
  33: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  34: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  35: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  36: std::thread::local::LocalKey<T>::with
  37: red_knot_python_semantic::semantic_index::visibility_constraints::VisibilityConstraints::evaluate
  38: red_knot_python_semantic::symbol::symbol_from_bindings_impl::{{closure}}
  39: red_knot_python_semantic::symbol::symbol_from_bindings_impl
  40: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_name_load
  41: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
  42: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_type_expression_no_store
  43: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_type_expression_no_store
  44: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_annotation_expression_impl
  45: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_parameters
  46: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
  47: <red_knot_python_semantic::types::infer::infer_deferred_types::Configuration_ as salsa::function::Configuration>::execute
  48: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  49: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  50: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  51: std::thread::local::LocalKey<T>::with
  52: red_knot_python_semantic::types::definition_expression_type
  53: red_knot_python_semantic::types::signatures::Signature::from_function
  54: red_knot_python_semantic::types::FunctionType::internal_signature
  55: <red_knot_python_semantic::types::FunctionType as red_knot_python_semantic::types::FunctionType::signature::InnerTrait_>::signature_
  56: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  57: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  58: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  59: std::thread::local::LocalKey<T>::with
  60: red_knot_python_semantic::types::Type::signatures
  61: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_call_expression
  62: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
  63: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region
  64: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
  65: <red_knot_python_semantic::types::infer::infer_expression_types::Configuration_ as salsa::function::Configuration>::execute
  66: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  67: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  68: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  69: std::thread::local::LocalKey<T>::with
  70: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_standalone_expression
  71: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region_definition
  72: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
  73: <red_knot_python_semantic::types::infer::infer_definition_types::Configuration_ as salsa::function::Configuration>::execute
  74: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  75: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  76: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  77: std::thread::local::LocalKey<T>::with
  78: red_knot_python_semantic::symbol::symbol_from_bindings_impl::{{closure}}
  79: red_knot_python_semantic::symbol::symbol_from_bindings_impl
  80: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_name_load
  81: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
  82: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_attribute_load
  83: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
  84: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
  85: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region
  86: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
  87: <red_knot_python_semantic::types::infer::infer_expression_types::Configuration_ as salsa::function::Configuration>::execute
  88: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  89: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  90: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  91: std::thread::local::LocalKey<T>::with
  92: <red_knot_python_semantic::types::infer::infer_expression_type::Configuration_ as salsa::function::Configuration>::execute
  93: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  94: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  95: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  96: std::thread::local::LocalKey<T>::with
  97: red_knot_python_semantic::semantic_index::visibility_constraints::VisibilityConstraints::evaluate
  98: red_knot_python_semantic::symbol::symbol_from_bindings_impl::{{closure}}
  99: red_knot_python_semantic::symbol::symbol_from_bindings_impl
 100: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_name_load
 101: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
 102: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_annotation_expression_impl
 103: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region
 104: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
 105: <red_knot_python_semantic::types::infer::infer_deferred_types::Configuration_ as salsa::function::Configuration>::execute
 106: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
 107: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
 108: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
 109: std::thread::local::LocalKey<T>::with
 110: red_knot_python_semantic::types::definition_expression_type
 111: red_knot_python_semantic::types::signatures::Signature::from_function
 112: red_knot_python_semantic::types::FunctionType::internal_signature
 113: <red_knot_python_semantic::types::FunctionType as red_knot_python_semantic::types::FunctionType::signature::InnerTrait_>::signature_
 114: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
 115: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
 116: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
 117: std::thread::local::LocalKey<T>::with
 118: red_knot_python_semantic::types::Type::signatures
 119: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_call_expression
 120: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
 121: core::ops::function::impls::<impl core::ops::function::FnMut<A> for &mut F>::call_mut
 122: <alloc::collections::vec_deque::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold
 123: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter::SpecFromIter<T,I>>::from_iter
 124: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_call_expression
 125: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
 126: core::iter::adapters::map::map_fold::{{closure}}
 127: <itertools::with_position::WithPosition<I> as core::iter::traits::iterator::Iterator>::fold
 128: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
 129: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region
 130: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
 131: <red_knot_python_semantic::types::infer::infer_expression_types::Configuration_ as salsa::function::Configuration>::execute
 132: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
 133: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
 134: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
 135: std::thread::local::LocalKey<T>::with
 136: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_standalone_expression
 137: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_body
 138: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region
 139: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
 140: <red_knot_python_semantic::types::infer::infer_scope_types::Configuration_ as salsa::function::Configuration>::execute
 141: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
 142: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
 143: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
 144: std::thread::local::LocalKey<T>::with
 145: <red_knot_python_semantic::types::check_types::Configuration_ as salsa::function::Configuration>::execute
 146: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
 147: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
 148: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
 149: std::thread::local::LocalKey<T>::with
 150: salsa::cancelled::Cancelled::catch
 151: ruff_db::panic::catch_unwind
 152: red_knot_project::check_file_impl
 153: <rayon_core::job::HeapJob<BODY> as rayon_core::job::Job>::execute
 154: rayon_core::registry::WorkerThread::wait_until_cold
 155: rayon_core::registry::ThreadBuilder::run
 156: std::sys::backtrace::__rust_begin_short_backtrace
 157: core::ops::function::FnOnce::call_once{{vtable.shim}}
 158: std::sys::pal::unix::thread::Thread::new::thread_start
 159: __pthread_cond_wait

info: query stacktrace:
   0: infer_definition_types(Id(61acbd))
             at crates/red_knot_python_semantic/src/types/infer.rs:144
             cycle heads: infer_expression_types(Id(60eaf3)) -> 0
   1: infer_expression_types(Id(60eaf5))
             at crates/red_knot_python_semantic/src/types/infer.rs:218
             cycle heads: infer_expression_types(Id(60ead4)) -> 0, infer_expression_type(Id(60eaf4)) -> 0
   2: infer_expression_type(Id(60eaf5))
             at crates/red_knot_python_semantic/src/types/infer.rs:274
   3: infer_deferred_types(Id(61acbc))
             at crates/red_knot_python_semantic/src/types/infer.rs:182
             cycle heads: infer_expression_types(Id(60ead4)) -> 0, infer_expression_type(Id(60eaf4)) -> 0
   4: signature_(Id(5a3ba1))
             at crates/red_knot_python_semantic/src/types.rs:6394
   5: infer_expression_types(Id(60eaf3))
             at crates/red_knot_python_semantic/src/types/infer.rs:218
             cycle heads: infer_expression_types(Id(60ead4)) -> 0
   6: infer_definition_types(Id(61acbe))
             at crates/red_knot_python_semantic/src/types/infer.rs:144
   7: infer_expression_types(Id(60eaf4))
             at crates/red_knot_python_semantic/src/types/infer.rs:218
             cycle heads: infer_expression_types(Id(60ead4)) -> 0
   8: infer_expression_type(Id(60eaf4))
             at crates/red_knot_python_semantic/src/types/infer.rs:274
   9: infer_deferred_types(Id(61ac9c))
             at crates/red_knot_python_semantic/src/types/infer.rs:182
             cycle heads: infer_expression_types(Id(60ead4)) -> 0
  10: signature_(Id(5a3ba0))
             at crates/red_knot_python_semantic/src/types.rs:6394
  11: infer_expression_types(Id(60ead4))
             at crates/red_knot_python_semantic/src/types/infer.rs:218
  12: infer_scope_types(Id(5f67ba))
             at crates/red_knot_python_semantic/src/types/infer.rs:117
  13: check_types(Id(103f))
             at crates/red_knot_python_semantic/src/types.rs:81
```

**check** without `RUST_BACKTRACE=1`

```
error: panic: Panicked at crates/red_knot_python_semantic/src/types/infer.rs:3705:14 when checking `__init__.py`: `expression should belong to this TypeInference region and TypeInferenceBuilder should have inferred a type for it`
info: This indicates a bug in Red Knot.
info: If you could open an issue at https://github.com/astral-sh/ruff/issues/new?title=%5Bred-knot%5D:%20panic we'd be very appreciative!
info: Platform: macos aarch64
info: Args: [".."]
info: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
info: query stacktrace:
   0: infer_definition_types(Id(5181c3))
             at crates/red_knot_python_semantic/src/types/infer.rs:144
             cycle heads: infer_expression_types(Id(51fc82)) -> 0
   1: infer_expression_types(Id(51fc84))
             at crates/red_knot_python_semantic/src/types/infer.rs:218
             cycle heads: infer_expression_types(Id(51fc63)) -> 0, infer_expression_type(Id(51fc83)) -> 0
   2: infer_expression_type(Id(51fc84))
             at crates/red_knot_python_semantic/src/types/infer.rs:274
   3: infer_deferred_types(Id(5181c2))
             at crates/red_knot_python_semantic/src/types/infer.rs:182
             cycle heads: infer_expression_types(Id(51fc63)) -> 0, infer_expression_type(Id(51fc83)) -> 0
   4: signature_(Id(4c9600))
             at crates/red_knot_python_semantic/src/types.rs:6394
   5: infer_expression_types(Id(51fc82))
             at crates/red_knot_python_semantic/src/types/infer.rs:218
             cycle heads: infer_expression_types(Id(51fc63)) -> 0
   6: infer_definition_types(Id(5181c4))
             at crates/red_knot_python_semantic/src/types/infer.rs:144
   7: infer_expression_types(Id(51fc83))
             at crates/red_knot_python_semantic/src/types/infer.rs:218
             cycle heads: infer_expression_types(Id(51fc63)) -> 0
   8: infer_expression_type(Id(51fc83))
             at crates/red_knot_python_semantic/src/types/infer.rs:274
   9: infer_deferred_types(Id(5181a2))
             at crates/red_knot_python_semantic/src/types/infer.rs:182
             cycle heads: infer_expression_types(Id(51fc63)) -> 0
  10: signature_(Id(4c95ff))
             at crates/red_knot_python_semantic/src/types.rs:6394
  11: infer_expression_types(Id(51fc63))
             at crates/red_knot_python_semantic/src/types/infer.rs:218
  12: infer_scope_types(Id(511945))
             at crates/red_knot_python_semantic/src/types/infer.rs:117
  13: check_types(Id(15cf))
             at crates/red_knot_python_semantic/src/types.rs:81
```


**mdtest** with `RUST_BACKTRACE=1`

```
unreachable.md - Unreachable code - Limitations of the current approach

  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485 panicked at crates/red_knot_python_semantic/src/types/infer.rs:279:5
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485 Test
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485    0: std::backtrace_rs::backtrace::libunwind::trace
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/../../backtrace/src/backtrace/libunwind.rs:117:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485    1: std::backtrace_rs::backtrace::trace_unsynchronized
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/../../backtrace/src/backtrace/mod.rs:66:14
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485    2: std::backtrace::Backtrace::create
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/backtrace.rs:331:13
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485    3: ruff_db::panic::install_hook::{{closure}}::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/astral/ruff/crates/ruff_db/src/panic.rs:81:34
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485    4: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/alloc/src/boxed.rs:1990:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485    5: std::panicking::rust_panic_with_hook
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panicking.rs:839:13
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485    6: std::panicking::begin_panic_handler::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panicking.rs:697:13
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485    7: std::sys::backtrace::__rust_end_short_backtrace
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/sys/backtrace.rs:168:18
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485    8: rust_begin_unwind
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panicking.rs:695:5
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485    9: core::panicking::panic_fmt
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/core/src/panicking.rs:75:14
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   10: <red_knot_python_semantic::types::infer::infer_expression_type::Configuration_ as salsa::function::Configuration>::execute::inner_
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:279:5
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   11: <red_knot_python_semantic::types::infer::infer_expression_type::Configuration_ as salsa::function::Configuration>::execute
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/components/salsa-macro-rules/src/setup_tracked_fn.rs:207:21
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   12: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/execute.rs:61:33
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   13: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/fetch.rs:197:20
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   14: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/fetch.rs:46:29
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   15: core::option::Option<T>::or_else
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   16: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/fetch.rs:44:33
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   17: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/fetch.rs:17:20
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   18: red_knot_python_semantic::types::infer::infer_expression_type::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/components/salsa-macro-rules/src/setup_tracked_fn.rs:368:25
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   19: salsa::attach::Attached::attach
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/attach.rs:73:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   20: salsa::attach::attach::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/attach.rs:97:13
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   21: std::thread::local::LocalKey<T>::try_with
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:310:12
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   22: std::thread::local::LocalKey<T>::with
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:274:15
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   23: salsa::attach::attach
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/attach.rs:95:5
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   24: red_knot_python_semantic::types::infer::infer_expression_type
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/components/salsa-macro-rules/src/setup_tracked_fn.rs:360:13
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   25: red_knot_python_semantic::semantic_index::visibility_constraints::VisibilityConstraints::analyze_single
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/semantic_index/visibility_constraints.rs:651:26
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   26: red_knot_python_semantic::semantic_index::visibility_constraints::VisibilityConstraints::evaluate
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/semantic_index/visibility_constraints.rs:551:19
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   27: red_knot_python_semantic::symbol::symbol_from_bindings_impl::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/symbol.rs:713:17
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   28: core::ops::function::impls::<impl core::ops::function::FnMut<A> for &mut F>::call_mut
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:294:13
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   29: core::iter::traits::iterator::Iterator::find_map::check::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2859:32
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   30: <core::iter::adapters::peekable::Peekable<I> as core::iter::traits::iterator::Iterator>::try_fold
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/peekable.rs:100:30
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   31: core::iter::traits::iterator::Iterator::find_map
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2865:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   32: <core::iter::adapters::filter_map::FilterMap<I,F> as core::iter::traits::iterator::Iterator>::next
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/filter_map.rs:64:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   33: red_knot_python_semantic::symbol::symbol_from_bindings_impl
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/symbol.rs:787:26
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   34: red_knot_python_semantic::symbol::symbol_from_bindings
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/symbol.rs:364:5
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   35: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_name_load
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:4814:13
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   36: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_name_expression
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:4982:34
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   37: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:3728:38
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   38: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_expression
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:3699:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   39: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_annotated_assignment_definition
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:3055:31
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   40: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region_definition
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:982:17
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   41: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:668:56
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   42: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:6739:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   43: <red_knot_python_semantic::types::infer::infer_definition_types::Configuration_ as salsa::function::Configuration>::execute::inner_
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:159:5
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   44: <red_knot_python_semantic::types::infer::infer_definition_types::Configuration_ as salsa::function::Configuration>::execute
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/components/salsa-macro-rules/src/setup_tracked_fn.rs:207:21
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   45: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/execute.rs:61:33
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   46: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/fetch.rs:197:20
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   47: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/fetch.rs:46:29
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   48: core::option::Option<T>::or_else
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   49: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/fetch.rs:44:33
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   50: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/fetch.rs:17:20
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   51: red_knot_python_semantic::types::infer::infer_definition_types::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/components/salsa-macro-rules/src/setup_tracked_fn.rs:368:25
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   52: salsa::attach::Attached::attach
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/attach.rs:73:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   53: salsa::attach::attach::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/attach.rs:97:13
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   54: std::thread::local::LocalKey<T>::try_with
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:310:12
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   55: std::thread::local::LocalKey<T>::with
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:274:15
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   56: salsa::attach::attach
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/attach.rs:95:5
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   57: red_knot_python_semantic::types::infer::infer_definition_types
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/components/salsa-macro-rules/src/setup_tracked_fn.rs:360:13
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   58: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_definition
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:1455:22
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   59: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_annotated_assignment_statement
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:2978:13
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   60: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_statement
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:1429:45
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   61: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_body
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:1413:13
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   62: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_if_statement
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:1934:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   63: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_statement
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:1424:44
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   64: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_body
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:1413:13
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   65: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_module
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:1215:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   66: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region_scope
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:679:17
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   67: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:667:46
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   68: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:6739:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   69: <red_knot_python_semantic::types::infer::infer_scope_types::Configuration_ as salsa::function::Configuration>::execute::inner_
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types/infer.rs:126:5
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   70: <red_knot_python_semantic::types::infer::infer_scope_types::Configuration_ as salsa::function::Configuration>::execute
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/components/salsa-macro-rules/src/setup_tracked_fn.rs:207:21
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   71: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/execute.rs:61:33
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   72: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/fetch.rs:197:20
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   73: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/fetch.rs:46:29
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   74: core::option::Option<T>::or_else
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   75: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/fetch.rs:44:33
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   76: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/fetch.rs:17:20
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   77: red_knot_python_semantic::types::infer::infer_scope_types::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/components/salsa-macro-rules/src/setup_tracked_fn.rs:368:25
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   78: salsa::attach::Attached::attach
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/attach.rs:73:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   79: salsa::attach::attach::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/attach.rs:97:13
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   80: std::thread::local::LocalKey<T>::try_with
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:310:12
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   81: std::thread::local::LocalKey<T>::with
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:274:15
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   82: salsa::attach::attach
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/attach.rs:95:5
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   83: red_knot_python_semantic::types::infer::infer_scope_types
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/components/salsa-macro-rules/src/setup_tracked_fn.rs:360:13
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   84: <red_knot_python_semantic::types::check_types::Configuration_ as salsa::function::Configuration>::execute::inner_
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./src/types.rs:91:22
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   85: <red_knot_python_semantic::types::check_types::Configuration_ as salsa::function::Configuration>::execute
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/components/salsa-macro-rules/src/setup_tracked_fn.rs:207:21
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   86: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/execute.rs:61:33
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   87: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/fetch.rs:197:20
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   88: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/fetch.rs:46:29
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   89: core::option::Option<T>::or_else
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   90: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/fetch.rs:44:33
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   91: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/function/fetch.rs:17:20
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   92: red_knot_python_semantic::types::check_types::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/components/salsa-macro-rules/src/setup_tracked_fn.rs:368:25
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   93: salsa::attach::Attached::attach
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/attach.rs:73:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   94: salsa::attach::attach::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/attach.rs:97:13
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   95: std::thread::local::LocalKey<T>::try_with
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:310:12
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   96: std::thread::local::LocalKey<T>::with
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:274:15
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   97: salsa::attach::attach
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/src/attach.rs:95:5
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   98: red_knot_python_semantic::types::check_types
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/af811a3/components/salsa-macro-rules/src/setup_tracked_fn.rs:360:13
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485   99: red_knot_test::run_test::{{closure}}::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/astral/ruff/crates/red_knot_test/src/lib.rs:319:58
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  100: std::panicking::try::do_call
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:587:40
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  101: ___rust_try
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  102: std::panicking::try
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:550:19
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  103: std::panic::catch_unwind
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panic.rs:358:14
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  104: ruff_db::panic::catch_unwind
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/astral/ruff/crates/ruff_db/src/panic.rs:112:18
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  105: red_knot_test::run_test::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/astral/ruff/crates/red_knot_test/src/lib.rs:319:42
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  106: core::ops::function::impls::<impl core::ops::function::FnMut<A> for &mut F>::call_mut
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:294:13
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  107: core::iter::traits::iterator::Iterator::find_map::check::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2859:32
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  108: <alloc::vec::into_iter::IntoIter<T,A> as core::iter::traits::iterator::Iterator>::try_fold
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/into_iter.rs:346:25
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  109: core::iter::traits::iterator::Iterator::find_map
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2865:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  110: <core::iter::adapters::filter_map::FilterMap<I,F> as core::iter::traits::iterator::Iterator>::next
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/filter_map.rs:64:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  111: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter_nested.rs:25:32
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  112: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter{{reify.shim}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter_nested.rs:19:5
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  113: alloc::vec::in_place_collect::<impl alloc::vec::spec_from_iter::SpecFromIter<T,I> for alloc::vec::Vec<T>>::from_iter
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/in_place_collect.rs:246:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  114: <alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3424:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  115: core::iter::traits::iterator::Iterator::collect
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:1971:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  116: red_knot_test::run_test
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/astral/ruff/crates/red_knot_test/src/lib.rs:301:30
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  117: red_knot_test::run
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/astral/ruff/crates/red_knot_test/src/lib.rs:67:32
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  118: mdtest::mdtest
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./tests/mdtest.rs:28:5
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  119: mdtest::mdtest__unreachable
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./tests/mdtest.rs:6:1
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  120: mdtest::mdtest__unreachable::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at ./tests/mdtest.rs:9:3
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  121: core::ops::function::FnOnce::call_once
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  122: core::ops::function::FnOnce::call_once
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/core/src/ops/function.rs:250:5
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  123: test::__rust_begin_short_backtrace
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/test/src/lib.rs:637:18
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  124: test::run_test_in_process::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/test/src/lib.rs:660:60
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  125: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/core/src/panic/unwind_safe.rs:272:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  126: std::panicking::try::do_call
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panicking.rs:587:40
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  127: std::panicking::try
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panicking.rs:550:19
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  128: std::panic::catch_unwind
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panic.rs:358:14
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  129: test::run_test_in_process
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/test/src/lib.rs:660:27
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  130: test::run_test::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/test/src/lib.rs:581:43
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  131: test::run_test::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/test/src/lib.rs:611:41
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  132: std::sys::backtrace::__rust_begin_short_backtrace
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/sys/backtrace.rs:152:18
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  133: std::thread::Builder::spawn_unchecked_::{{closure}}::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/thread/mod.rs:559:17
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  134: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/core/src/panic/unwind_safe.rs:272:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  135: std::panicking::try::do_call
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panicking.rs:587:40
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  136: std::panicking::try
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panicking.rs:550:19
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  137: std::panic::catch_unwind
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panic.rs:358:14
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  138: std::thread::Builder::spawn_unchecked_::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/thread/mod.rs:557:30
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  139: core::ops::function::FnOnce::call_once{{vtable.shim}}
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/core/src/ops/function.rs:250:5
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  140: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/alloc/src/boxed.rs:1976:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  141: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/alloc/src/boxed.rs:1976:9
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  142: std::sys::pal::unix::thread::Thread::new::thread_start
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/sys/pal/unix/thread.rs:106:17
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485  143: __pthread_cond_wait
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485 
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485 query stacktrace:
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485    0: infer_expression_type(Id(10e0))
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at crates/red_knot_python_semantic/src/types/infer.rs:274
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485    1: infer_definition_types(Id(4e87))
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at crates/red_knot_python_semantic/src/types/infer.rs:144
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485    2: infer_scope_types(Id(800))
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at crates/red_knot_python_semantic/src/types/infer.rs:117
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485    3: check_types(Id(0))
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485              at crates/red_knot_python_semantic/src/types.rs:81
  crates/red_knot_python_semantic/resources/mdtest/unreachable.md:485 
```

---

_Label `red-knot` added by @MichaReiser on 2025-04-30 07:30_

---

_Comment by @github-actions[bot] on 2025-04-30 07:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-04-30 07:43_

---

_Review requested from @carljm by @MichaReiser on 2025-04-30 07:43_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-30 07:43_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-30 07:43_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-30 07:43_

---

_Comment by @github-actions[bot] on 2025-04-30 07:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@sharkdp reviewed on 2025-04-30 08:05_

---

_Review comment by @sharkdp on `crates/red_knot_project/src/lib.rs`:634 on 2025-04-30 08:05_

That message might be slightly confusing to users in the presence of the query backtrace that we seem to show unconditionally (right below this message).

Maybe
```suggestion
                            "run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information",
```

---

_@sharkdp approved on 2025-04-30 08:05_

Thank you!

---

_Closed by @MichaReiser on 2025-04-30 08:20_

---

_Reopened by @MichaReiser on 2025-04-30 08:20_

---

_Merged by @MichaReiser on 2025-04-30 08:26_

---

_Closed by @MichaReiser on 2025-04-30 08:26_

---

_Branch deleted on 2025-04-30 08:26_

---
