---
number: 17600
title: "[red-knot] Cyclic generic class: `access to field whilst the value is being initialized`"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - ty
assignees: []
created_at: 2025-04-24T09:05:49Z
updated_at: 2025-04-30T15:31:07Z
url: https://github.com/astral-sh/ruff/issues/17600
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Cyclic generic class: `access to field whilst the value is being initialized`

---

_Issue opened by @MichaReiser on 2025-04-24 09:05_

```py
class Foo[T: Foo, U: (Foo, Foo)]:
    pass
            
```

```
thread '<unnamed>' panicked at /Users/micha/astral/salsa/src/tracked_struct.rs:847:21:
access to field whilst the value is being initialized
stack backtrace:
   0: rust_begin_unwind
             at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panicking.rs:695:5
   1: core::panicking::panic_fmt
             at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/core/src/panicking.rs:75:14
   2: salsa::tracked_struct::Value<C>::read_lock
             at /Users/micha/astral/salsa/src/tracked_struct.rs:847:21
   3: salsa::tracked_struct::IngredientImpl<C>::untracked_field
             at /Users/micha/astral/salsa/src/tracked_struct.rs:726:9
   4: red_knot_python_semantic::types::_::<impl red_knot_python_semantic::types::TypeVarInstance>::default_ty
             at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_struct.rs:282:38
   5: red_knot_python_semantic::types::generics::GenericContext::default_specialization::{{closure}}
             at ./crates/red_knot_python_semantic/src/types/generics.rs:87:28
   6: core::iter::adapters::map::map_fold::{{closure}}
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/map.rs:88:28
   7: <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/slice/iter/macros.rs:232:27
   8: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/map.rs:128:9
   9: core::iter::traits::iterator::Iterator::for_each
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:800:9
  10: alloc::vec::Vec<T,A>::extend_trusted
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3565:17
  11: <alloc::vec::Vec<T,A> as alloc::vec::spec_extend::SpecExtend<T,I>>::spec_extend
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_extend.rs:29:9
  12: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter_nested.rs:62:9
  13: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter::SpecFromIter<T,I>>::from_iter
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter.rs:34:9
  14: <alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3424:9
  15: core::iter::traits::iterator::Iterator::collect
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:1971:9
  16: alloc::boxed::iter::<impl core::iter::traits::collect::FromIterator<I> for alloc::boxed::Box<[I]>>::from_iter
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/boxed/iter.rs:144:9
  17: core::iter::traits::iterator::Iterator::collect
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:1971:9
  18: red_knot_python_semantic::types::generics::GenericContext::default_specialization
             at ./crates/red_knot_python_semantic/src/types/generics.rs:84:21
  19: red_knot_python_semantic::types::class::ClassLiteralType::default_specialization
             at ./crates/red_knot_python_semantic/src/types/class.rs:538:38
  20: red_knot_python_semantic::types::Type::in_type_expression
             at ./crates/red_knot_python_semantic/src/types.rs:4391:41
  21: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_type_expression_no_store
             at ./crates/red_knot_python_semantic/src/types/infer.rs:6844:43
  22: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_type_expression
             at ./crates/red_knot_python_semantic/src/types/infer.rs:6798:18
  23: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_typevar_definition::{{closure}}
             at ./crates/red_knot_python_semantic/src/types/infer.rs:2184:41
  24: core::iter::adapters::map::map_fold::{{closure}}
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/map.rs:88:28
  25: <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/slice/iter/macros.rs:232:27
  26: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/map.rs:128:9
  27: core::iter::traits::iterator::Iterator::for_each
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:800:9
  28: alloc::vec::Vec<T,A>::extend_trusted
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3565:17
  29: <alloc::vec::Vec<T,A> as alloc::vec::spec_extend::SpecExtend<T,I>>::spec_extend
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_extend.rs:29:9
  30: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter_nested.rs:62:9
  31: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter::SpecFromIter<T,I>>::from_iter
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter.rs:34:9
  32: <alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3424:9
  33: core::iter::traits::iterator::Iterator::collect
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:1971:9
  34: alloc::boxed::iter::<impl core::iter::traits::collect::FromIterator<I> for alloc::boxed::Box<[I]>>::from_iter
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/boxed/iter.rs:144:9
  35: core::iter::traits::iterator::Iterator::collect
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:1971:9
  36: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_typevar_definition
             at ./crates/red_knot_python_semantic/src/types/infer.rs:2183:25
  37: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region_definition
             at ./crates/red_knot_python_semantic/src/types/infer.rs:1018:17
  38: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region
             at ./crates/red_knot_python_semantic/src/types/infer.rs:667:56
  39: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
             at ./crates/red_knot_python_semantic/src/types/infer.rs:6597:9
  40: <red_knot_python_semantic::types::infer::infer_definition_types::Configuration_ as salsa::function::Configuration>::execute::inner_
             at ./crates/red_knot_python_semantic/src/types/infer.rs:159:5
  41: <red_knot_python_semantic::types::infer::infer_definition_types::Configuration_ as salsa::function::Configuration>::execute
             at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_fn.rs:206:21
  42: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /Users/micha/astral/salsa/src/function/execute.rs:61:33
  43: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /Users/micha/astral/salsa/src/function/fetch.rs:210:20
  44: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /Users/micha/astral/salsa/src/function/fetch.rs:46:29
  45: core::option::Option<T>::or_else
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
  46: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /Users/micha/astral/salsa/src/function/fetch.rs:44:33
  47: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /Users/micha/astral/salsa/src/function/fetch.rs:17:20
  48: red_knot_python_semantic::types::infer::infer_definition_types::{{closure}}
             at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_fn.rs:367:25
  49: salsa::attach::Attached::attach
             at /Users/micha/astral/salsa/src/attach.rs:73:9
  50: salsa::attach::attach::{{closure}}
             at /Users/micha/astral/salsa/src/attach.rs:97:13
  51: std::thread::local::LocalKey<T>::try_with
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:310:12
  52: std::thread::local::LocalKey<T>::with
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:274:15
  53: salsa::attach::attach
             at /Users/micha/astral/salsa/src/attach.rs:95:5
  54: red_knot_python_semantic::types::infer::infer_definition_types
             at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_fn.rs:359:13
  55: red_knot_python_semantic::types::declaration_type
             at ./crates/red_knot_python_semantic/src/types.rs:117:21
  56: red_knot_python_semantic::types::generics::GenericContext::variable_from_type_param
             at ./crates/red_knot_python_semantic/src/types/generics.rs:44:21
  57: red_knot_python_semantic::types::generics::GenericContext::from_type_params::{{closure}}
             at ./crates/red_knot_python_semantic/src/types/generics.rs:30:38
  58: core::ops::function::impls::<impl core::ops::function::FnMut<A> for &mut F>::call_mut
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:294:13
  59: <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::find_map
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/slice/iter/macros.rs:319:38
  60: <core::iter::adapters::filter_map::FilterMap<I,F> as core::iter::traits::iterator::Iterator>::next
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/filter_map.rs:64:9
  61: alloc::vec::Vec<T,A>::extend_desugared
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3532:35
  62: <alloc::vec::Vec<T,A> as alloc::vec::spec_extend::SpecExtend<T,I>>::spec_extend
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_extend.rs:19:9
  63: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter_nested.rs:42:9
  64: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter::SpecFromIter<T,I>>::from_iter
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter.rs:34:9
  65: <alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3424:9
  66: core::iter::traits::iterator::Iterator::collect
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:1971:9
  67: alloc::boxed::iter::<impl core::iter::traits::collect::FromIterator<I> for alloc::boxed::Box<[I]>>::from_iter
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/boxed/iter.rs:144:9
  68: core::iter::traits::iterator::Iterator::collect
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:1971:9
  69: red_knot_python_semantic::types::generics::GenericContext::from_type_params
             at ./crates/red_knot_python_semantic/src/types/generics.rs:28:35
  70: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_class_definition::{{closure}}
             at ./crates/red_knot_python_semantic/src/types/infer.rs:1829:13
  71: core::option::Option<T>::map
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:1119:29
  72: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_class_definition
             at ./crates/red_knot_python_semantic/src/types/infer.rs:1828:31
  73: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region_definition
             at ./crates/red_knot_python_semantic/src/types/infer.rs:960:45
  74: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region
             at ./crates/red_knot_python_semantic/src/types/infer.rs:667:56
  75: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
             at ./crates/red_knot_python_semantic/src/types/infer.rs:6597:9
  76: <red_knot_python_semantic::types::infer::infer_definition_types::Configuration_ as salsa::function::Configuration>::execute::inner_
             at ./crates/red_knot_python_semantic/src/types/infer.rs:159:5
  77: <red_knot_python_semantic::types::infer::infer_definition_types::Configuration_ as salsa::function::Configuration>::execute
             at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_fn.rs:206:21
  78: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /Users/micha/astral/salsa/src/function/execute.rs:61:33
  79: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /Users/micha/astral/salsa/src/function/fetch.rs:210:20
  80: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /Users/micha/astral/salsa/src/function/fetch.rs:46:29
  81: core::option::Option<T>::or_else
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
  82: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /Users/micha/astral/salsa/src/function/fetch.rs:44:33
  83: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /Users/micha/astral/salsa/src/function/fetch.rs:17:20
  84: red_knot_python_semantic::types::infer::infer_definition_types::{{closure}}
             at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_fn.rs:367:25
  85: salsa::attach::Attached::attach
             at /Users/micha/astral/salsa/src/attach.rs:73:9
  86: salsa::attach::attach::{{closure}}
             at /Users/micha/astral/salsa/src/attach.rs:97:13
  87: std::thread::local::LocalKey<T>::try_with
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:310:12
  88: std::thread::local::LocalKey<T>::with
             at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:274:15
  89: salsa::attach::attach
             at /Users/micha/astral/salsa/src/attach.rs:95:5
  90: red_knot_python_semantic::types::infer::infer_definition_types
             at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_fn.rs:359:13
  91: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_definition
             at ./crates/red_knot_python_semantic/src/types/infer.rs:1454:22
  92: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_class_definition_statement
             at ./crates/red_knot_python_semantic/src/types/infer.rs:1781:9
  93: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_statement
             at ./crates/red_knot_python_semantic/src/types/infer.rs:1419:43
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
Rayon: detected unexpected panic; aborting
fish: Job 1, 'RUST_BACKTRACE=1 RAYON_NUM_THRE…' terminated by signal SIGABRT (Abort)
```

Found thanks to the CPython (Argument Clinic) mypy primer test.

---

_Label `bug` added by @MichaReiser on 2025-04-24 09:05_

---

_Label `red-knot` added by @MichaReiser on 2025-04-24 09:05_

---

_Added to milestone `Red Knot Alpha` by @MichaReiser on 2025-04-24 09:05_

---

_Comment by @MichaReiser on 2025-04-24 09:08_

This is most likely another instance where we access a tracked struct from a previous cycle that has been reclaimed

---

_Comment by @MichaReiser on 2025-04-24 09:22_

@carljm is this an issue that you're already aware and working on or should I take a look?

---

_Assigned to @MichaReiser by @MichaReiser on 2025-04-24 10:04_

---

_Referenced in [astral-sh/ruff#17602](../../astral-sh/ruff/pulls/17602.md) on 2025-04-24 10:04_

---

_Comment by @carljm on 2025-04-24 10:28_

I'm not currently working on this issue and would be happy if you can look at it!

---

_Comment by @MichaReiser on 2025-04-24 10:41_

Funny, if we simplify the case to 

```py
class Foo[T: Foo]:
    pass
            
```

it then exits with too many cycles. But I think this is an issue @carljm already brought up before

---

_Comment by @MichaReiser on 2025-04-24 10:53_

I suspect that this is another instance where we end up reusing freed tracked struct. The issue is slightly different to the other salsa bug that I fixed and I don't fully understand it yet but the test panics when dereferencing a `TypeVarInstance` (tracked) from a `GenericContext` and the logs show that this `TypeVarInstance` was freed before. 

I'm unsure if this is a cycle specific problem or a more general problem where an interned references a now freed tracked struct.

---

_Comment by @carljm on 2025-04-24 10:55_

I noticed last week that we have cases of interned structs referencing tracked structs, and it's not really clear to me whether this is safe in general. 

---

_Comment by @carljm on 2025-04-24 10:58_

@dcreager also mentioned in that discussion that he thinks TypeVarInstance should possibly be interned instead anyway. 

---

_Comment by @MichaReiser on 2025-04-24 11:23_

I don't think it's unsafe outside cycles because you can't get a reference to the interned struct without going through the queries `maybe_changed_after`. At least, I'm unable to come up with an example where a query has a cached interned that points to a tracked struct that was discarded in a previous revision

However, I think (I haven't yet been able to come up with an example) it is possible with cycle handling where a query that doesn't participate in the cycle creates the interned struct. But the query that created the tracked struct (which is referenced in the interned) gets re-executed as part of the cycle.

---

_Comment by @carljm on 2025-04-24 11:32_

It's not clear to me how a query that doesn't participate in a cycle could get access to a tracked structs from within the cycle iteration. The intention of the design, anyway, is that provisional query results don't escape from the cycle, and there are some tests for this. It's certainly possible there is a missed case. 

---

_Comment by @MichaReiser on 2025-04-24 11:37_

These are the logs, in case anyone is interested (or sees something obvious)

```
2025-04-24T11:36:37.915063Z DEBUG ruff_db::files: Syncing all files
2025-04-24T11:36:37.915346Z TRACE ruff_db::files: Adding file '/src/mdtest_snippet.py'
2025-04-24T11:36:37.915754Z  INFO red_knot_python_semantic::program: Python version: Python 3.9, platform: linux
2025-04-24T11:36:37.915800Z DEBUG red_knot_python_semantic::module_resolver::resolver: Adding first-party search path '/src'
2025-04-24T11:36:37.915816Z DEBUG red_knot_python_semantic::module_resolver::resolver: Using vendored stdlib
2025-04-24T11:36:37.917008Z TRACE red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: parsed_module(Id(0)) } }
2025-04-24T11:36:37.917105Z TRACE parsed_module: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: source_text(Id(0)) } } file=File(System("/src/mdtest_snippet.py"))
2025-04-24T11:36:37.917539Z TRACE red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: check_types(Id(0)) } }
2025-04-24T11:36:37.917578Z DEBUG check_types: red_knot_python_semantic::types: Checking file '/src/mdtest_snippet.py' file=File(System("/src/mdtest_snippet.py"))
2025-04-24T11:36:37.917647Z TRACE check_types: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: semantic_index(Id(0)) } } file=File(System("/src/mdtest_snippet.py"))
2025-04-24T11:36:37.918405Z TRACE check_types: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: infer_scope_types(Id(800)) } } file=File(System("/src/mdtest_snippet.py"))
2025-04-24T11:36:37.918545Z TRACE check_types:infer_scope_types: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: infer_definition_types(Id(c02)) } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py"))
2025-04-24T11:36:37.918579Z TRACE check_types:infer_scope_types:infer_definition_types: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: infer_definition_types(Id(c00)) } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py")) range=6..9 file=File(System("/src/mdtest_snippet.py"))
2025-04-24T11:36:37.918706Z TRACE check_types:infer_scope_types:infer_definition_types:infer_definition_types:symbol: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: symbol_table(Id(800)) } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py")) range=6..9 file=File(System("/src/mdtest_snippet.py")) range=10..11 file=File(System("/src/mdtest_snippet.py")) name="Foo"
2025-04-24T11:36:37.918865Z TRACE check_types:infer_scope_types:infer_definition_types:infer_definition_types:symbol: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: DidInternValue { key: Configuration(Id(1000)), revision: R1 } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py")) range=6..9 file=File(System("/src/mdtest_snippet.py")) range=10..11 file=File(System("/src/mdtest_snippet.py")) name="Foo"
2025-04-24T11:36:37.918908Z TRACE check_types:infer_scope_types:infer_definition_types:infer_definition_types:symbol: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: symbol_by_id(Id(1000)) } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py")) range=6..9 file=File(System("/src/mdtest_snippet.py")) range=10..11 file=File(System("/src/mdtest_snippet.py")) name="Foo"
2025-04-24T11:36:37.918941Z TRACE check_types:infer_scope_types:infer_definition_types:infer_definition_types:symbol: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: use_def_map(Id(800)) } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py")) range=6..9 file=File(System("/src/mdtest_snippet.py")) range=10..11 file=File(System("/src/mdtest_snippet.py")) name="Foo"
2025-04-24T11:36:37.919085Z TRACE check_types:infer_scope_types:infer_definition_types:infer_definition_types: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: suppressions(Id(0)) } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py")) range=6..9 file=File(System("/src/mdtest_snippet.py")) range=10..11 file=File(System("/src/mdtest_snippet.py"))
2025-04-24T11:36:37.919196Z TRACE check_types:infer_scope_types:infer_definition_types: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: infer_definition_types(Id(c01)) } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py")) range=6..9 file=File(System("/src/mdtest_snippet.py"))
2025-04-24T11:36:37.919405Z TRACE check_types:infer_scope_types:infer_definition_types:infer_definition_types: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: DidInternValue { key: UnionType(Id(1800)), revision: R1 } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py")) range=6..9 file=File(System("/src/mdtest_snippet.py")) range=18..19 file=File(System("/src/mdtest_snippet.py"))
2025-04-24T11:36:37.919549Z TRACE check_types:infer_scope_types:infer_definition_types: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: DidInternValue { key: GenericContext(Id(1c00)), revision: R1 } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py")) range=6..9 file=File(System("/src/mdtest_snippet.py"))
2025-04-24T11:36:37.919603Z TRACE check_types:infer_scope_types:infer_definition_types: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: DidInternValue { key: GenericClass(Id(2000)), revision: R1 } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py")) range=6..9 file=File(System("/src/mdtest_snippet.py"))
2025-04-24T11:36:37.919619Z TRACE check_types:infer_scope_types: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillIterateCycle { database_key: infer_definition_types(Id(c02)), iteration_count: 1, fell_back: false } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py"))
2025-04-24T11:36:37.919690Z TRACE check_types:infer_scope_types:infer_definition_types: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: infer_definition_types(Id(c00)) } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py")) range=6..9 file=File(System("/src/mdtest_snippet.py"))
2025-04-24T11:36:37.919722Z TRACE check_types:infer_scope_types:infer_definition_types:infer_definition_types:symbol: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: symbol_by_id(Id(1000)) } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py")) range=6..9 file=File(System("/src/mdtest_snippet.py")) range=10..11 file=File(System("/src/mdtest_snippet.py")) name="Foo"
2025-04-24T11:36:37.919877Z TRACE check_types:infer_scope_types:infer_definition_types:infer_definition_types: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: DidInternValue { key: Specialization(Id(2400)), revision: R1 } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py")) range=6..9 file=File(System("/src/mdtest_snippet.py")) range=10..11 file=File(System("/src/mdtest_snippet.py"))
2025-04-24T11:36:37.919942Z TRACE check_types:infer_scope_types:infer_definition_types:infer_definition_types: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: DidInternValue { key: GenericAlias(Id(2800)), revision: R1 } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py")) range=6..9 file=File(System("/src/mdtest_snippet.py")) range=10..11 file=File(System("/src/mdtest_snippet.py"))
2025-04-24T11:36:37.920032Z TRACE check_types:infer_scope_types:infer_definition_types: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillDiscardStaleOutput { execute_key: infer_definition_types(Id(c00)), output_key: TypeVarInstance(Id(1400)) } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py")) range=6..9 file=File(System("/src/mdtest_snippet.py"))
2025-04-24T11:36:37.920043Z TRACE check_types:infer_scope_types:infer_definition_types: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: DidDiscard { key: TypeVarInstance(Id(1400)) } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py")) range=6..9 file=File(System("/src/mdtest_snippet.py"))
2025-04-24T11:36:37.920068Z TRACE check_types:infer_scope_types:infer_definition_types: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: infer_definition_types(Id(c01)) } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py")) range=6..9 file=File(System("/src/mdtest_snippet.py"))
test mdtest__cycle_panic ... FAILED

```


and the backtrace

```
 crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10 panicked at /Users/micha/astral/salsa/src/tracked_struct.rs:847:21
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10 access to field whilst the value is being initialized
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10    0: std::backtrace_rs::backtrace::libunwind::trace
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/../../backtrace/src/backtrace/libunwind.rs:117:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10    1: std::backtrace_rs::backtrace::trace_unsynchronized
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/../../backtrace/src/backtrace/mod.rs:66:14
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10    2: std::backtrace::Backtrace::create
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/backtrace.rs:331:13
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10    3: ruff_db::panic::install_hook::{{closure}}::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/ruff/crates/ruff_db/src/panic.rs:50:29
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10    4: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/alloc/src/boxed.rs:1990:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10    5: std::panicking::rust_panic_with_hook
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panicking.rs:839:13
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10    6: std::panicking::begin_panic_handler::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panicking.rs:697:13
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10    7: std::sys::backtrace::__rust_end_short_backtrace
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/sys/backtrace.rs:168:18
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10    8: rust_begin_unwind
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panicking.rs:695:5
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10    9: core::panicking::panic_fmt
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/core/src/panicking.rs:75:14
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   10: salsa::tracked_struct::Value<C>::read_lock
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/tracked_struct.rs:847:21
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   11: salsa::tracked_struct::IngredientImpl<C>::untracked_field
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/tracked_struct.rs:726:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   12: red_knot_python_semantic::types::_::<impl red_knot_python_semantic::types::TypeVarInstance>::default_ty
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_struct.rs:282:38
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   13: red_knot_python_semantic::types::generics::GenericContext::default_specialization::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/generics.rs:87:28
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   14: core::iter::adapters::map::map_fold::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/map.rs:88:28
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   15: <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/slice/iter/macros.rs:232:27
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   16: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/map.rs:128:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   17: core::iter::traits::iterator::Iterator::for_each
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:800:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   18: alloc::vec::Vec<T,A>::extend_trusted
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3565:17
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   19: <alloc::vec::Vec<T,A> as alloc::vec::spec_extend::SpecExtend<T,I>>::spec_extend
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_extend.rs:29:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   20: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter_nested.rs:62:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   21: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter::SpecFromIter<T,I>>::from_iter
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter.rs:34:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   22: <alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3424:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   23: core::iter::traits::iterator::Iterator::collect
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:1971:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   24: alloc::boxed::iter::<impl core::iter::traits::collect::FromIterator<I> for alloc::boxed::Box<[I]>>::from_iter
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/boxed/iter.rs:144:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   25: core::iter::traits::iterator::Iterator::collect
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:1971:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   26: red_knot_python_semantic::types::generics::GenericContext::default_specialization
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/generics.rs:84:21
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   27: red_knot_python_semantic::types::class::ClassLiteralType::default_specialization
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/class.rs:538:38
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   28: red_knot_python_semantic::types::Type::in_type_expression
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types.rs:4391:41
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   29: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_type_expression_no_store
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:6844:43
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   30: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_type_expression
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:6798:18
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   31: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_typevar_definition::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:2184:41
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   32: core::iter::adapters::map::map_fold::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/map.rs:88:28
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   33: <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/slice/iter/macros.rs:232:27
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   34: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/map.rs:128:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   35: core::iter::traits::iterator::Iterator::for_each
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:800:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   36: alloc::vec::Vec<T,A>::extend_trusted
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3565:17
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   37: <alloc::vec::Vec<T,A> as alloc::vec::spec_extend::SpecExtend<T,I>>::spec_extend
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_extend.rs:29:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   38: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter_nested.rs:62:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   39: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter::SpecFromIter<T,I>>::from_iter
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter.rs:34:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   40: <alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3424:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   41: core::iter::traits::iterator::Iterator::collect
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:1971:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   42: alloc::boxed::iter::<impl core::iter::traits::collect::FromIterator<I> for alloc::boxed::Box<[I]>>::from_iter
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/boxed/iter.rs:144:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   43: core::iter::traits::iterator::Iterator::collect
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:1971:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   44: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_typevar_definition
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:2183:25
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   45: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region_definition
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:1018:17
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   46: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:667:56
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   47: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:6597:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   48: <red_knot_python_semantic::types::infer::infer_definition_types::Configuration_ as salsa::function::Configuration>::execute::inner_
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:159:5
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   49: <red_knot_python_semantic::types::infer::infer_definition_types::Configuration_ as salsa::function::Configuration>::execute
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_fn.rs:206:21
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   50: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/execute.rs:61:33
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   51: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/fetch.rs:209:20
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   52: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/fetch.rs:46:29
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   53: core::option::Option<T>::or_else
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   54: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/fetch.rs:44:33
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   55: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/fetch.rs:17:20
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   56: red_knot_python_semantic::types::infer::infer_definition_types::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_fn.rs:367:25
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   57: salsa::attach::Attached::attach
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/attach.rs:73:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   58: salsa::attach::attach::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/attach.rs:97:13
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   59: std::thread::local::LocalKey<T>::try_with
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:310:12
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   60: std::thread::local::LocalKey<T>::with
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:274:15
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   61: salsa::attach::attach
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/attach.rs:95:5
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   62: red_knot_python_semantic::types::infer::infer_definition_types
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_fn.rs:359:13
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   63: red_knot_python_semantic::types::declaration_type
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types.rs:117:21
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   64: red_knot_python_semantic::types::generics::GenericContext::variable_from_type_param
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/generics.rs:44:21
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   65: red_knot_python_semantic::types::generics::GenericContext::from_type_params::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/generics.rs:30:38
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   66: core::ops::function::impls::<impl core::ops::function::FnMut<A> for &mut F>::call_mut
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:294:13
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   67: <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::find_map
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/slice/iter/macros.rs:319:38
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   68: <core::iter::adapters::filter_map::FilterMap<I,F> as core::iter::traits::iterator::Iterator>::next
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/filter_map.rs:64:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   69: alloc::vec::Vec<T,A>::extend_desugared
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3532:35
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   70: <alloc::vec::Vec<T,A> as alloc::vec::spec_extend::SpecExtend<T,I>>::spec_extend
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_extend.rs:19:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   71: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter_nested.rs:42:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   72: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter::SpecFromIter<T,I>>::from_iter
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter.rs:34:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   73: <alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3424:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   74: core::iter::traits::iterator::Iterator::collect
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:1971:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   75: alloc::boxed::iter::<impl core::iter::traits::collect::FromIterator<I> for alloc::boxed::Box<[I]>>::from_iter
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/boxed/iter.rs:144:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   76: core::iter::traits::iterator::Iterator::collect
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:1971:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   77: red_knot_python_semantic::types::generics::GenericContext::from_type_params
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/generics.rs:28:35
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   78: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_class_definition::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:1829:13
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   79: core::option::Option<T>::map
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:1119:29
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   80: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_class_definition
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:1828:31
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   81: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region_definition
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:960:45
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   82: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:667:56
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   83: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:6597:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   84: <red_knot_python_semantic::types::infer::infer_definition_types::Configuration_ as salsa::function::Configuration>::execute::inner_
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:159:5
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   85: <red_knot_python_semantic::types::infer::infer_definition_types::Configuration_ as salsa::function::Configuration>::execute
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_fn.rs:206:21
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   86: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/execute.rs:61:33
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   87: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/fetch.rs:209:20
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   88: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/fetch.rs:46:29
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   89: core::option::Option<T>::or_else
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   90: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/fetch.rs:44:33
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   91: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/fetch.rs:17:20
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   92: red_knot_python_semantic::types::infer::infer_definition_types::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_fn.rs:367:25
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   93: salsa::attach::Attached::attach
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/attach.rs:73:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   94: salsa::attach::attach::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/attach.rs:97:13
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   95: std::thread::local::LocalKey<T>::try_with
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:310:12
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   96: std::thread::local::LocalKey<T>::with
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:274:15
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   97: salsa::attach::attach
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/attach.rs:95:5
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   98: red_knot_python_semantic::types::infer::infer_definition_types
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_fn.rs:359:13
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10   99: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_definition
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:1454:22
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  100: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_class_definition_statement
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:1781:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  101: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_statement
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:1419:43
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  102: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_body
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:1412:13
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  103: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_module
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:1214:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  104: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region_scope
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:678:17
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  105: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:666:46
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  106: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:6597:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  107: <red_knot_python_semantic::types::infer::infer_scope_types::Configuration_ as salsa::function::Configuration>::execute::inner_
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types/infer.rs:126:5
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  108: <red_knot_python_semantic::types::infer::infer_scope_types::Configuration_ as salsa::function::Configuration>::execute
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_fn.rs:206:21
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  109: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/execute.rs:61:33
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  110: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/fetch.rs:209:20
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  111: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/fetch.rs:46:29
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  112: core::option::Option<T>::or_else
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  113: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/fetch.rs:44:33
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  114: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/fetch.rs:17:20
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  115: red_knot_python_semantic::types::infer::infer_scope_types::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_fn.rs:367:25
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  116: salsa::attach::Attached::attach
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/attach.rs:73:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  117: salsa::attach::attach::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/attach.rs:97:13
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  118: std::thread::local::LocalKey<T>::try_with
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:310:12
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  119: std::thread::local::LocalKey<T>::with
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:274:15
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  120: salsa::attach::attach
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/attach.rs:95:5
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  121: red_knot_python_semantic::types::infer::infer_scope_types
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_fn.rs:359:13
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  122: <red_knot_python_semantic::types::check_types::Configuration_ as salsa::function::Configuration>::execute::inner_
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./src/types.rs:90:22
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  123: <red_knot_python_semantic::types::check_types::Configuration_ as salsa::function::Configuration>::execute
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_fn.rs:206:21
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  124: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/execute.rs:61:33
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  125: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/fetch.rs:209:20
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  126: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/fetch.rs:46:29
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  127: core::option::Option<T>::or_else
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  128: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/fetch.rs:44:33
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  129: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/function/fetch.rs:17:20
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  130: red_knot_python_semantic::types::check_types::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_fn.rs:367:25
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  131: salsa::attach::Attached::attach
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/attach.rs:73:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  132: salsa::attach::attach::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/attach.rs:97:13
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  133: std::thread::local::LocalKey<T>::try_with
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:310:12
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  134: std::thread::local::LocalKey<T>::with
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:274:15
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  135: salsa::attach::attach
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/src/attach.rs:95:5
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  136: red_knot_python_semantic::types::check_types
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/salsa/components/salsa-macro-rules/src/setup_tracked_fn.rs:359:13
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  137: red_knot_test::run_test::{{closure}}::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/ruff/crates/red_knot_test/src/lib.rs:318:58
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  138: std::panicking::try::do_call
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:587:40
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  139: ___rust_try
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  140: std::panicking::try
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:550:19
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  141: std::panic::catch_unwind
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panic.rs:358:14
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  142: ruff_db::panic::catch_unwind
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/ruff/crates/ruff_db/src/panic.rs:83:18
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  143: red_knot_test::run_test::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/ruff/crates/red_knot_test/src/lib.rs:318:42
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  144: core::ops::function::impls::<impl core::ops::function::FnMut<A> for &mut F>::call_mut
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:294:13
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  145: core::iter::traits::iterator::Iterator::find_map::check::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2859:32
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  146: <alloc::vec::into_iter::IntoIter<T,A> as core::iter::traits::iterator::Iterator>::try_fold
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/into_iter.rs:346:25
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  147: core::iter::traits::iterator::Iterator::find_map
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2865:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  148: <core::iter::adapters::filter_map::FilterMap<I,F> as core::iter::traits::iterator::Iterator>::next
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/filter_map.rs:64:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  149: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter_nested.rs:25:32
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  150: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter{{reify.shim}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter_nested.rs:19:5
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  151: alloc::vec::in_place_collect::<impl alloc::vec::spec_from_iter::SpecFromIter<T,I> for alloc::vec::Vec<T>>::from_iter
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/in_place_collect.rs:246:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  152: <alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3424:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  153: core::iter::traits::iterator::Iterator::collect
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:1971:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  154: red_knot_test::run_test
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/ruff/crates/red_knot_test/src/lib.rs:300:30
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  155: red_knot_test::run
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/astral/ruff/crates/red_knot_test/src/lib.rs:66:32
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  156: mdtest::mdtest
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./tests/mdtest.rs:28:5
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  157: mdtest::mdtest__cycle_panic
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./tests/mdtest.rs:6:1
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  158: mdtest::mdtest__cycle_panic::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at ./tests/mdtest.rs:9:3
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  159: core::ops::function::FnOnce::call_once
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /Users/micha/.rustup/toolchains/1.86-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  160: core::ops::function::FnOnce::call_once
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/core/src/ops/function.rs:250:5
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  161: test::__rust_begin_short_backtrace
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/test/src/lib.rs:637:18
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  162: test::run_test_in_process::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/test/src/lib.rs:660:60
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  163: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/core/src/panic/unwind_safe.rs:272:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  164: std::panicking::try::do_call
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panicking.rs:587:40
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  165: std::panicking::try
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panicking.rs:550:19
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  166: std::panic::catch_unwind
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panic.rs:358:14
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  167: test::run_test_in_process
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/test/src/lib.rs:660:27
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  168: test::run_test::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/test/src/lib.rs:581:43
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  169: test::run_test::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/test/src/lib.rs:611:41
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  170: std::sys::backtrace::__rust_begin_short_backtrace
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/sys/backtrace.rs:152:18
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  171: std::thread::Builder::spawn_unchecked_::{{closure}}::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/thread/mod.rs:559:17
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  172: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/core/src/panic/unwind_safe.rs:272:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  173: std::panicking::try::do_call
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panicking.rs:587:40
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  174: std::panicking::try
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panicking.rs:550:19
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  175: std::panic::catch_unwind
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panic.rs:358:14
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  176: std::thread::Builder::spawn_unchecked_::{{closure}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/thread/mod.rs:557:30
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  177: core::ops::function::FnOnce::call_once{{vtable.shim}}
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/core/src/ops/function.rs:250:5
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  178: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/alloc/src/boxed.rs:1976:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  179: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/alloc/src/boxed.rs:1976:9
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  180: std::sys::pal::unix::thread::Thread::new::thread_start
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10              at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/sys/pal/unix/thread.rs:106:17
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10  181: __pthread_deallocate
  crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10 

```

---

_Comment by @MichaReiser on 2025-04-24 12:49_

Okay, this is interesting.

`infer_definition_types(id(c00))` creates the tracked struct `TypeVarInstance(id(1400))` the first time it gets executed

```
2025-04-24T12:39:54.643151Z DEBUG check_types:infer_scope_types:infer_definition_types: salsa::function::execute: infer_definition_types(Id(c00)): execute: result.revisions = QueryRevisions {
    changed_at: R1,
    durability: Durability(
        0,
    ),
    origin: Derived(
        QueryEdges {
            input_outputs: [
                Input(
                    Definition(Id(c00)),
                ),
                Input(
                    Definition.file(Id(c00)),
                ),
                Input(
                    File.path(Id(0)),
                ),
                Input(
                    semantic_index(Id(0)),
                ),
                Input(
                    ScopeId(Id(801)),
                ),
                Input(
                    ScopeId(Id(800)),
                ),
                Input(
                    symbol_table(Id(800)),
                ),
                Input(
                    Configuration(Id(1000)),
                ),
                Input(
                    symbol_by_id(Id(1000)),
                ),
                Input(
                    suppressions(Id(0)),
                ),
                Output(
                    TypeVarInstance(Id(1400)),
                ),
            ],
        },
    ),
    tracked_struct_ids: IdentityMap {
        map: {
            Identity {
                ingredient_index: IngredientIndex(
                    24,
                ),
                hash: 3216879360148310997,
                disambiguator: Disambiguator(
                    0,
                ),
            }: Id(1400),
        },
    },
    accumulated: None,
    accumulated_inputs: AtomicInputAccumulatedValues(
        false,
    ),
    verified_final: false,
    cycle_heads: CycleHeads(
        [
            CycleHead {
                database_key_index: infer_definition_types(Id(c02)),
                iteration_count: 0,
            },
        ],
    ),
} file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py")) range=6..9 file=File(System("/src/mdtest_snippet.py"))
```

Further down in the log, the query gets reexecuted

```
2025-04-24T12:39:54.644737Z DEBUG check_types:infer_scope_types:infer_definition_types: salsa::function::execute: infer_definition_types(Id(c00)): execute: result.revisions = QueryRevisions {
    changed_at: R1,
    durability: Durability(
        0,
    ),
    origin: Derived(
        QueryEdges {
            input_outputs: [
                Input(
                    Definition(Id(c00)),
                ),
                Input(
                    Definition.file(Id(c00)),
                ),
                Input(
                    File.path(Id(0)),
                ),
                Input(
                    semantic_index(Id(0)),
                ),
                Input(
                    ScopeId(Id(801)),
                ),
                Input(
                    ScopeId(Id(800)),
                ),
                Input(
                    symbol_table(Id(800)),
                ),
                Input(
                    Configuration(Id(1000)),
                ),
                Input(
                    symbol_by_id(Id(1000)),
                ),
                Input(
                    TypeVarInstance(Id(1400)),
                ),
                Input(
                    TypeVarInstance(Id(1401)),
                ),
                Input(
                    Specialization(Id(2400)),
                ),
                Input(
                    GenericAlias(Id(2800)),
                ),
                Output(
                    TypeVarInstance(Id(1402)),
                ),
            ],
        },
    ),
    tracked_struct_ids: IdentityMap {
        map: {
            Identity {
                ingredient_index: IngredientIndex(
                    24,
                ),
                hash: 971972673530254868,
                disambiguator: Disambiguator(
                    0,
                ),
            }: Id(1402),
            Identity {
                ingredient_index: IngredientIndex(
                    24,
                ),
                hash: 3216879360148310997,
                disambiguator: Disambiguator(
                    0,
                ),
            }: Id(1400),
        },
    },
    accumulated: None,
    accumulated_inputs: AtomicInputAccumulatedValues(
        false,
    ),
    verified_final: false,
    cycle_heads: CycleHeads(
        [
            CycleHead {
                database_key_index: infer_definition_types(Id(c02)),
                iteration_count: 1,
            },
        ],
    ),
} file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snippet.py")) range=6..9 file=File(System("/src/mdtest_snippet.py"))
2025-04-24T12:39:54.644769Z TRACE check_types:infer_scope_types:infer_definition_types: red_knot_test::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillDiscardStaleOutput { execute_key: infer_definition_types(Id(c00)), output_key: TypeVarInstance(Id(1400)) } } file=File(System("/src/mdtest_snippet.py")) scope=Id(800) file=File(System("/src/mdtest_snip
```

What's interesting is that `TypeVarInstance(Id(1400))` now is an input and no longer listed as an output... which why it gets reclaimed immediately after the query executes which now invalidates all interned values that reference it

---

_Comment by @carljm on 2025-04-24 12:56_

Hmm. It seems like that part is maybe all correct? When the query re-executes, we have to create a new typevar instance, because the class (which is part of the typevar as a bound) has changed.

The question is, how does any other query (even another query within the cycle when re-executing) get access to the previous interned value that is holding onto the old tracked struct? Is it possibly an issue of the old tracked struct and the new one comparing equal to each other, so a later interned-new call gets back the previous interned value when it should create a new one?

---

_Comment by @MichaReiser on 2025-04-24 12:58_

> Hmm. It seems like that part is maybe all correct? When the query re-executes, we have to create a new typevar instance, because the class (which is part of the typevar as a bound) has changed.

I don't think this is correct because the query also depends on `TypeVarInstance(Id(1400)),` which gets released, leaving the query itself in an invalidate state. I don't think a query should be able to depend on its own outputs?

---

_Comment by @MichaReiser on 2025-04-24 13:17_

I wonder if the problem is due to the reads in `apply_optional_specialization`

https://github.com/astral-sh/ruff/blob/e897f37911ccc195299607dead61ed111a936664/crates/red_knot_python_semantic/src/types/class.rs#L522-L526

---

_Comment by @carljm on 2025-04-24 13:20_

It seems like a query potentially depending on its own output (from a previous cycle iteration) is something that might be hard to prevent with tracked structs in a cycle?

---

_Comment by @MichaReiser on 2025-04-24 13:24_

> It seems like a query potentially depending on its own output (from a previous cycle iteration) is something that might be hard to prevent with tracked structs in a cycle?

Possibly. But we'd have to detect this because the basic promise of tracked structs is that they have an owning query. If that's violated, everything false apart...

---

_Comment by @MichaReiser on 2025-04-25 10:29_

Salsa PR https://github.com/salsa-rs/salsa/pull/817

Note, we should no longer see this issue in Red Knot, now that `TypeVarInstance` is an interned struct.

---

_Closed by @MichaReiser on 2025-04-30 15:31_

---
