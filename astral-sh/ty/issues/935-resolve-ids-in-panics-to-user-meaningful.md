```yaml
number: 935
title: Resolve ids in panics to user-meaningful information
type: issue
state: closed
author: lf-
labels: []
assignees: []
created_at: 2025-08-04T15:14:06Z
updated_at: 2025-08-04T17:33:33Z
url: https://github.com/astral-sh/ty/issues/935
synced_at: 2026-01-12T15:54:24Z
```

# Resolve ids in panics to user-meaningful information

---

_@lf-_

Say you're having ty get sad about a recursive type (#256): the panic message is quite unhelpful as it doesn't tell you anything that could lead to figuring out the provenance of the problem objects:

```
error[panic]: Panicked at /private/tmp/nix-build-ty-0.0.1-alpha.15.drv-0/ty-0.0.1-alpha.15-vendor/salsa-0.23.0/src/f
unction/fetch.rs:165:25 when checking `/Users/jade/lix/lix/tests/functional2/lang/test_lang_infra.py`: `dependency g
raph cycle when querying PEP695TypeAliasType < 'db >::value_type_(Id(7000)), set cycle_fn/cycle_initial to fixpoint 
iterate.
Query stack:
[
    check_file_impl(Id(c00)),
    infer_scope_types(Id(1000)),
    infer_definition_types(Id(1400)),
    Type < 'db >::member_lookup_with_policy_(Id(2400)),
    place_by_id(Id(2800)),
    infer_definition_types(Id(1403)),
    PEP695TypeAliasType < 'db >::value_type_(Id(7000)),
    infer_scope_types(Id(5af6)),
    place_by_id(Id(2811)),
    infer_definition_types(Id(614b)),
    infer_definition_types(Id(6141)),
    Type < 'db >::member_lookup_with_policy_(Id(240a)),
    place_by_id(Id(2810)),
    infer_definition_types(Id(61e3)),
    infer_definition_types(Id(61d3)),
    file_to_module(Id(1c31)),
    ClassLiteral < 'db >::explicit_bases_(Id(480d)),
    infer_deferred_types(Id(17fe)),
    infer_definition_types(Id(175f)),
    infer_expression_types(Id(2ce7)),
    ClassLiteral < 'db >::explicit_bases_(Id(480b)),
    infer_deferred_types(Id(575b)),
    infer_definition_types(Id(456f)),
]`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appre
ciative!
info: Platform: macos aarch64
info: Version: 0.0.1-alpha.15
info: Args: ["ty", "check", "--python", "/nix/store/vvmy9g9v59x8nkx4vlhiq2glrd4j2h4x-python3-3.12.10-env/bin/python"
, "lang/test_lang_infra.py"]
info: Backtrace:
   0: std::backtrace::Backtrace::create
   1: ruff_db::panic::install_hook::{{closure}}::{{closure}}
   2: std::panicking::rust_panic_with_hook
   3: std::panicking::begin_panic_handler::{{closure}}
   4: std::sys::backtrace::__rust_end_short_backtrace
   5: __rustc::rust_begin_unwind
   6: core::panicking::panic_fmt
   7: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold::{{closure}}
   8: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold_with_retry
   9: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  10: ty_python_semantic::types::PEP695TypeAliasType::value_type
  11: ty_python_semantic::types::Type::in_type_expression
  12: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_type_expression_no_store
  13: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_type_expression_no_store
  14: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_type_expression_no_store
  15: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_type_expression
  16: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_explicit_class_specialization
  17: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_subscript_type_expression
  18: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_annotation_expression_impl
  19: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_region_scope
  20: ty_python_semantic::types::infer::TypeInferenceBuilder::finish
  21: <ty_python_semantic::types::infer::infer_scope_types::Configuration_ as salsa::function::Configuration>::execu
te
  22: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  23: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold_with_retry
  24: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  25: std::thread::local::LocalKey<T>::with
  26: ty_python_semantic::types::definition_expression_type
  27: <ty_python_semantic::types::PEP695TypeAliasType as ty_python_semantic::types::PEP695TypeAliasType::value_type:
:InnerTrait_>::value_type_
  28: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  29: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold_with_retry
  30: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  31: ty_python_semantic::types::PEP695TypeAliasType::value_type
  32: ty_python_semantic::types::Type::in_type_expression
  33: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_annotation_expression_impl
  34: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_region_definition
  35: ty_python_semantic::types::infer::TypeInferenceBuilder::finish
  36: <ty_python_semantic::types::infer::infer_definition_types::Configuration_ as salsa::function::Configuration>::
execute
  37: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  38: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold_with_retry
  39: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  40: std::thread::local::LocalKey<T>::with
  41: <core::iter::adapters::peekable::Peekable<I> as core::iter::traits::iterator::Iterator>::try_fold
  42: ty_python_semantic::place::place_from_declarations_impl
  43: <ty_python_semantic::place::place_by_id::Configuration_ as salsa::function::Configuration>::execute
  44: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  45: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold_with_retry
  46: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  47: std::thread::local::LocalKey<T>::with
  48: ty_python_semantic::place::symbol_impl
  49: ty_python_semantic::place::imported_symbol
  50: ty_python_semantic::types::ModuleLiteralType::static_member
  51: <ty_python_semantic::types::Type as ty_python_semantic::types::Type::member_lookup_with_policy::InnerTrait_>::
member_lookup_with_policy_
  52: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  53: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold_with_retry
  54: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  55: ty_python_semantic::types::Type::member_lookup_with_policy
  56: ty_python_semantic::types::Type::member
  57: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_import_from_definition
  58: ty_python_semantic::types::infer::TypeInferenceBuilder::finish
  59: <ty_python_semantic::types::infer::infer_definition_types::Configuration_ as salsa::function::Configuration>::
execute
  60: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  61: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold_with_retry
  62: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  63: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_body
  64: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_region_scope
  65: ty_python_semantic::types::infer::TypeInferenceBuilder::finish
  66: <ty_python_semantic::types::infer::infer_scope_types::Configuration_ as salsa::function::Configuration>::execu
te
  67: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  68: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold_with_retry
  69: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  70: std::thread::local::LocalKey<T>::with
  71: ty_python_semantic::types::check_types
  72: salsa::cancelled::Cancelled::catch
  73: <ty_project::check_file_impl::Configuration_ as salsa::function::Configuration>::execute
  74: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  75: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold_with_retry
  76: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  77: std::thread::local::LocalKey<T>::with
  78: rayon_core::scope::ScopeBase::execute_job_closure
  79: <rayon_core::job::HeapJob<BODY> as rayon_core::job::Job>::execute
  80: rayon_core::registry::WorkerThread::wait_until_cold
  81: rayon_core::scope::ScopeBase::complete
  82: rayon_core::scope::scope::{{closure}}
  82: rayon_core::scope::scope::{{closure}}
  83: ty_project::Project::check
  84: salsa::cancelled::Cancelled::catch
  85: <rayon_core::job::HeapJob<BODY> as rayon_core::job::Job>::execute
  86: rayon_core::registry::WorkerThread::wait_until_cold
  87: rayon_core::registry::ThreadBuilder::run
  88: std::sys::backtrace::__rust_begin_short_backtrace
  89: core::ops::function::FnOnce::call_once{{vtable.shim}}
  90: std::sys::pal::unix::thread::Thread::new::thread_start
  91: __pthread_cond_wait

info: query stacktrace:
   0: infer_scope_types(Id(5af6))
             at crates/ty_python_semantic/src/types/infer.rs:129
   1: PEP695TypeAliasType < 'db >::value_type_(Id(7000))
             at crates/ty_python_semantic/src/types.rs:7724
   2: infer_definition_types(Id(1403))
             at crates/ty_python_semantic/src/types/infer.rs:158
   3: place_by_id(Id(2800))
             at crates/ty_python_semantic/src/place.rs:638
   4: Type < 'db >::member_lookup_with_policy_(Id(2400))
             at crates/ty_python_semantic/src/types.rs:596
   5: infer_definition_types(Id(1400))
             at crates/ty_python_semantic/src/types/infer.rs:158
   6: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:129
   7: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:482
```

In particular, the IDs listed here aren't resolved to anything (and don't give any idea of what file the error is from, nor the identifier associated with the problem object). It *is* plausible to want to avoid this data getting near the hot path because of it being useless the majority of the time typechecking valid code, but I would assume that there's more data available than lands in panic messages.

---

_Comment by @lf- on 2025-08-04 15:26_

This may be unfixable (or salsa's problem) since, for ??? reasons, the salsa backtrace type is opaque (!!) https://docs.rs/salsa/latest/salsa/struct.Backtrace.html

I'm not sure how the printing works for these or if it's possible to set up the printing to be more useful.

---

_Comment by @MichaReiser on 2025-08-04 17:30_

I agree that the panic message isn't very helpful for users. But the proper fix here is to support recursive types. We print the IDs (or mainly query names) because they give us an idea of where the panic happens. I understand that they aren't useful to users, but they are also not really intended to be (the same as a normal backtrace).

So I think the solution here is to support recursive types and reduce the chance that a user will ever see a panic message like this

---

_Closed by @MichaReiser on 2025-08-04 17:33_

---
