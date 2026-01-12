```yaml
number: 18530
title: "[ty] Optimize `Type::has_relation_to()`"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - performance
  - ty
assignees: []
base: main
head: alex/optimize-subtyping
created_at: 2025-06-07T09:11:46Z
updated_at: 2025-06-12T10:52:51Z
url: https://github.com/astral-sh/ruff/pull/18530
synced_at: 2026-01-12T15:56:20Z
```

# [ty] Optimize `Type::has_relation_to()`

---

_@AlexWaygood_

## Summary

`Type::is_fully_static` is called from `Type::has_relation_to` to check whether the relation applies to a given pair of types. `Type::is_fully_static` deeply recurses into types to check whether a type is fully static or not. But `Type::has_relation_to` often calls itself recursively, and it shouldn't be necessary when doing a recursive call of `Type::has_relation_to` to check _again_ whether the pair of types is fully static or not: that should already have been checked by the top-most call.

This PR changes the `TypeRelation` enum so that if we're doing a subtyping check through `Type::has_relation_to`, we "remember" whether we've already checked that this pair of types is fully static or not. If so, we just return the cached result.

Locally I see a 3% speedup from these changes on the `many_tuple_assignments` benchmark. [Codspeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Foptimize-subtyping) sees maybe a tiny improvement on that benchmark in CI, but it's small enough to be dismissed as noise :-(

I still think this might be worth doing anyway: the current design _looks_ extremely inefficient, even if it isn't actually costing us much in any of our current benchmarks.

In the future, we can also unify `Type::is_equivalent_to()` and `Type::is_gradual_equivalent_to()` the same way that I unified `Type::is_assignable_to` and `Type::is_subtype_of`. This would allow us to pass the `TypeRelation` enum into those methods too, which would mean we wouldn't have to redundantly check whether a pair of types was both fully static if that had already been checked by an outer call. (These methods are both called by `Type::has_relation_to`.)

## Test Plan

All existing tests pass and there is no mypy_primer fallout.


---

_Label `performance` added by @AlexWaygood on 2025-06-07 09:11_

---

_Label `ty` added by @AlexWaygood on 2025-06-07 09:11_

---

_Comment by @github-actions[bot] on 2025-06-07 09:15_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6952 on 2025-06-07 09:37_

another advantage of this change is that it means that we can add checks like this (which means the method is now sound if it's called directly, not just if it's called from `Type::has_relation_to()`) without worrying about these checks slowing down `Type::has_relation_to()` (if the method is called from `Type::has_relation_to()`, the result of the check will already be cached)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7594 on 2025-06-07 09:37_

an unrelated optimisation I threw in here: comparisons between two `UnionType`s should be very cheap since they're both `u32`s under the hood, so it makes sense to do that first

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7710 on 2025-06-07 09:37_

another unrelated optimisation

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:558 on 2025-06-07 09:39_

I'm not totally sure why, but the changes in this PR meant that we started panicking here. I sort-of prefer it without the `unwrap()` calls anyway, though, as it means we don't need the SAFETY comment. Cc. @dhruvmanila

---

_@AlexWaygood reviewed on 2025-06-07 09:39_

---

_Marked ready for review by @AlexWaygood on 2025-06-07 09:39_

---

_Review requested from @carljm by @AlexWaygood on 2025-06-07 09:39_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-07 09:39_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-07 09:39_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:558 on 2025-06-09 08:49_

I don't think this should panic because this is a subtyping check and it should only be called for fully static types. If it's panicking, then the caller is violating the invariant, the `.unwrap` is to make sure the invariant is followed. Can you provide an example where it's panicking on this branch using `.unwrap`? 

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:6952 on 2025-06-09 09:03_

Are there any use cases of calling `has_relation_to` for individual type structs like this and others? If not, we could avoid this and maintain the invariant that these checks should go through `Type`. I'm not sure what benefit would there be to check for relations on specific type. I'm guessing that the only reason these methods exists on individual type structs is to avoid increasing the complexity in `Type::has_relation_to` method.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:6831 on 2025-06-09 09:08_

We could use [`OnceCell`](https://doc.rust-lang.org/stable/std/cell/struct.OnceCell.html) here to avoid making the `relation` parameter as `mut` across the methods. This would also make `None` unrepresentable.

---

_@dhruvmanila reviewed on 2025-06-09 09:08_

---

_@AlexWaygood reviewed on 2025-06-09 10:40_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6952 on 2025-06-09 10:40_

Hmm, I suppose I could turn these `if` checks into `debug_assert!`s that check we're not passing `TypeRelation::Subtyping` into a `has_relation_to()` call for non-fully-static-types?

We do have places around the codebase that call `ClassType::is_subclass_of`... but that's a bit of an "odd one out", because `ClassType`s don't really have assignability or subtyping relations to each other. Other types that wrap `ClassType`s have assignability/subtyping relations to each other (`ClassLiteral` types, `GenericAlias` types, `NominalInstance` types, `ProtocolInstance` types and `SubclassOf` types). But while you can determine whether one `SubclassOf` type is a subtype of another just by checking whether the wrapped `ClassType` is a subclass of the other wrapped `ClassType`, you can't do the same to determine whether one `ClassLiteral` types is a subtype of another `ClassLiteral` type (one `ClassLiteral` type is only a subtype of another `ClassLiteral` type if the two types are actually the _same_ type!).

---

_@AlexWaygood reviewed on 2025-06-09 10:52_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6831 on 2025-06-09 10:52_

Nice! I made this change in https://github.com/astral-sh/ruff/pull/18530/commits/df64227aa27ce395118485cad3df1eab69d2aec8

One small disadvantage is that this means `ApplicabilityCheck` and `TypeRelation` can no longer be `Copy`, but that's fairly easily worked around.

---

_@AlexWaygood reviewed on 2025-06-09 10:55_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:558 on 2025-06-09 10:55_

<details>

```
failures:

---- mdtest__type_properties_is_subtype_of stdout ----

is_subtype_of.md - Subtype relation - Callable - Class literals - Classes with `__init… (5c313ee5578fff58)

  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321 panicked at crates/ty_python_semantic/src/types/signatures.rs:555:52
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321 called `Option::unwrap()` on a `None` value
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321    0: std::backtrace_rs::backtrace::libunwind::trace
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/../../backtrace/src/backtrace/libunwind.rs:117:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321    1: std::backtrace_rs::backtrace::trace_unsynchronized
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/../../backtrace/src/backtrace/mod.rs:66:14
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321    2: std::backtrace::Backtrace::create
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/backtrace.rs:331:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321    3: ruff_db::panic::install_hook::{{closure}}::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/dev/ruff/crates/ruff_db/src/panic.rs:91:34
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321    4: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/alloc/src/boxed.rs:1980:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321    5: std::panicking::rust_panic_with_hook
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/panicking.rs:841:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321    6: std::panicking::begin_panic_handler::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/panicking.rs:699:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321    7: std::sys::backtrace::__rust_end_short_backtrace
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/sys/backtrace.rs:168:18
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321    8: __rustc::rust_begin_unwind
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/panicking.rs:697:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321    9: core::panicking::panic_fmt
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/core/src/panicking.rs:75:14
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   10: core::panicking::panic
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/core/src/panicking.rs:145:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   11: core::option::unwrap_failed
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/core/src/option.rs:2015:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   12: core::option::Option<T>::unwrap
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:978:21
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   13: ty_python_semantic::types::signatures::Signature::is_subtype_of::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/signatures.rs:555:46
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   14: ty_python_semantic::types::signatures::Signature::is_assignable_to_impl
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/signatures.rs:746:33
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   15: ty_python_semantic::types::signatures::Signature::is_subtype_of
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/signatures.rs:553:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   16: ty_python_semantic::types::signatures::CallableSignature::is_subtype_of::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/signatures.rs:120:48
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   17: ty_python_semantic::types::signatures::CallableSignature::has_relation_to_impl
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/signatures.rs:150:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   18: ty_python_semantic::types::signatures::CallableSignature::is_subtype_of
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/signatures.rs:117:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   19: ty_python_semantic::types::signatures::CallableSignature::has_relation_to
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/signatures.rs:108:44
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   20: ty_python_semantic::types::CallableType::has_relation_to
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types.rs:7085:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   21: ty_python_semantic::types::Type::has_relation_to
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types.rs:1289:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   22: ty_python_semantic::types::Type::has_relation_to
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types.rs:1267:74
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   23: ty_python_semantic::types::Type::has_relation_to::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types.rs:1143:33
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   24: <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::all
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/slice/iter/macros.rs:291:25
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   25: ty_python_semantic::types::Type::has_relation_to
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types.rs:1140:40
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   26: ty_python_semantic::types::Type::has_relation_to
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types.rs:1361:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   27: ty_python_semantic::types::Type::is_subtype_of
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types.rs:1057:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   28: ty_python_semantic::types::call::bind::Bindings::evaluate_known_cases
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/call/bind.rs:568:37
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   29: ty_python_semantic::types::call::bind::Bindings::check_types
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/call/bind.rs:126:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   30: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_call_expression
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/infer.rs:5185:15
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   31: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/infer.rs:4498:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   32: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_expression
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/infer.rs:4443:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   33: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_unary_expression
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/infer.rs:6273:28
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   34: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/infer.rs:4488:45
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   35: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_expression
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/infer.rs:4443:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   36: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_argument_type
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/infer.rs:4427:50
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   37: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_argument_types::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/infer.rs:4412:26
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   38: ty_python_semantic::types::call::arguments::CallArgumentTypes::new::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/call/arguments.rs:89:36
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   39: core::iter::adapters::map::map_fold::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/map.rs:88:28
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   40: <core::iter::adapters::enumerate::Enumerate<I> as core::iter::traits::iterator::Iterator>::fold::enumerate::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/enumerate.rs:107:27
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   41: core::iter::adapters::copied::copy_fold::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/copied.rs:30:22
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   42: <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/slice/iter/macros.rs:255:27
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   43: <core::iter::adapters::copied::Copied<I> as core::iter::traits::iterator::Iterator>::fold
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/copied.rs:75:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   44: <core::iter::adapters::enumerate::Enumerate<I> as core::iter::traits::iterator::Iterator>::fold
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/enumerate.rs:113:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   45: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/map.rs:128:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   46: core::iter::traits::iterator::Iterator::for_each
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:800:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   47: alloc::vec::Vec<T,A>::extend_trusted
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3579:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   48: <alloc::vec::Vec<T,A> as alloc::vec::spec_extend::SpecExtend<T,I>>::spec_extend
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_extend.rs:29:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   49: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter_nested.rs:62:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   50: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter::SpecFromIter<T,I>>::from_iter
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter.rs:34:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   51: <alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3438:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   52: core::iter::traits::iterator::Iterator::collect
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:1985:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   53: ty_python_semantic::types::call::arguments::CallArgumentTypes::new
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/call/arguments.rs:86:21
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   54: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_argument_types
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/infer.rs:4401:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   55: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_call_expression
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/infer.rs:5183:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   56: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/infer.rs:4498:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   57: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_expression
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/infer.rs:4443:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   58: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_statement
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/infer.rs:1986:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   59: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_body
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/infer.rs:1977:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   60: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_module
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/infer.rs:1734:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   61: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_region_scope
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/infer.rs:780:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   62: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_region
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/infer.rs:769:46
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   63: ty_python_semantic::types::infer::TypeInferenceBuilder::finish
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/infer.rs:8189:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   64: <ty_python_semantic::types::infer::infer_scope_types::Configuration_ as salsa::function::Configuration>::execute::inner_
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types/infer.rs:146:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   65: <ty_python_semantic::types::infer::infer_scope_types::Configuration_ as salsa::function::Configuration>::execute
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/components/salsa-macro-rules/src/setup_tracked_fn.rs:204:21
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   66: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_query
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/execute.rs:283:25
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   67: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_maybe_iterate
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/execute.rs:139:50
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   68: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/execute.rs:90:48
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   69: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:255:20
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   70: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold_with_retry
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:95:20
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   71: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:52:21
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   72: core::option::Option<T>::or_else
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   73: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:49:33
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   74: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:21:20
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   75: ty_python_semantic::types::infer::infer_scope_types::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/components/salsa-macro-rules/src/setup_tracked_fn.rs:362:25
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   76: salsa::attach::Attached::attach
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/attach.rs:79:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   77: salsa::attach::attach::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/attach.rs:103:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   78: std::thread::local::LocalKey<T>::try_with
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:311:12
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   79: std::thread::local::LocalKey<T>::with
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:275:15
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   80: salsa::attach::attach
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/attach.rs:101:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   81: ty_python_semantic::types::infer::infer_scope_types
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/components/salsa-macro-rules/src/setup_tracked_fn.rs:354:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   82: <ty_python_semantic::types::check_types::Configuration_ as salsa::function::Configuration>::execute::inner_
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./src/types.rs:98:22
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   83: <ty_python_semantic::types::check_types::Configuration_ as salsa::function::Configuration>::execute
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/components/salsa-macro-rules/src/setup_tracked_fn.rs:204:21
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   84: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_query
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/execute.rs:283:25
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   85: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/execute.rs:44:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   86: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:255:20
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   87: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold_with_retry
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:95:20
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   88: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:52:21
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   89: core::option::Option<T>::or_else
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   90: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:49:33
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   91: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:21:20
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   92: ty_python_semantic::types::check_types::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/components/salsa-macro-rules/src/setup_tracked_fn.rs:362:25
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   93: salsa::attach::Attached::attach
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/attach.rs:79:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   94: salsa::attach::attach::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/attach.rs:103:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   95: std::thread::local::LocalKey<T>::try_with
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:311:12
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   96: std::thread::local::LocalKey<T>::with
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:275:15
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   97: salsa::attach::attach
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/attach.rs:101:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   98: ty_python_semantic::types::check_types
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/components/salsa-macro-rules/src/setup_tracked_fn.rs:354:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321   99: ty_test::run_test::{{closure}}::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/dev/ruff/crates/ty_test/src/lib.rs:312:58
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  100: std::panicking::try::do_call
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:589:40
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  101: ___rust_try
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  102: std::panicking::try
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:552:19
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  103: std::panic::catch_unwind
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panic.rs:359:14
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  104: ruff_db::panic::catch_unwind
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/dev/ruff/crates/ruff_db/src/panic.rs:122:18
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  105: ty_test::run_test::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/dev/ruff/crates/ty_test/src/lib.rs:312:42
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  106: core::ops::function::impls::<impl core::ops::function::FnMut<A> for &mut F>::call_mut
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:294:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  107: core::iter::traits::iterator::Iterator::find_map::check::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2873:32
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  108: <alloc::vec::into_iter::IntoIter<T,A> as core::iter::traits::iterator::Iterator>::try_fold
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/into_iter.rs:346:25
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  109: core::iter::traits::iterator::Iterator::find_map
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2879:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  110: <core::iter::adapters::filter_map::FilterMap<I,F> as core::iter::traits::iterator::Iterator>::next
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/filter_map.rs:64:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  111: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter_nested.rs:25:32
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  112: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter{{reify.shim}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter_nested.rs:19:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  113: alloc::vec::in_place_collect::<impl alloc::vec::spec_from_iter::SpecFromIter<T,I> for alloc::vec::Vec<T>>::from_iter
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/in_place_collect.rs:246:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  114: <alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3438:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  115: core::iter::traits::iterator::Iterator::collect
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:1985:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  116: ty_test::run_test
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/dev/ruff/crates/ty_test/src/lib.rs:294:30
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  117: ty_test::run
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/dev/ruff/crates/ty_test/src/lib.rs:72:32
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  118: mdtest::mdtest
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./tests/mdtest.rs:28:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  119: mdtest::mdtest__type_properties_is_subtype_of
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./tests/mdtest.rs:6:1
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  120: mdtest::mdtest__type_properties_is_subtype_of::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at ./tests/mdtest.rs:9:3
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  121: core::ops::function::FnOnce::call_once
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  122: core::ops::function::FnOnce::call_once
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/core/src/ops/function.rs:250:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  123: test::__rust_begin_short_backtrace
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/test/src/lib.rs:638:18
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  124: test::run_test_in_process::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/test/src/lib.rs:661:60
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  125: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/core/src/panic/unwind_safe.rs:272:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  126: std::panicking::try::do_call
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/panicking.rs:589:40
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  127: std::panicking::try
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/panicking.rs:552:19
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  128: std::panic::catch_unwind
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/panic.rs:359:14
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  129: test::run_test_in_process
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/test/src/lib.rs:661:27
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  130: test::run_test::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/test/src/lib.rs:582:43
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  131: test::run_test::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/test/src/lib.rs:612:41
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  132: std::sys::backtrace::__rust_begin_short_backtrace
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/sys/backtrace.rs:152:18
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  133: std::thread::Builder::spawn_unchecked_::{{closure}}::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/thread/mod.rs:559:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  134: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/core/src/panic/unwind_safe.rs:272:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  135: std::panicking::try::do_call
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/panicking.rs:589:40
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  136: std::panicking::try
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/panicking.rs:552:19
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  137: std::panic::catch_unwind
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/panic.rs:359:14
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  138: std::thread::Builder::spawn_unchecked_::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/thread/mod.rs:557:30
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  139: core::ops::function::FnOnce::call_once{{vtable.shim}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/core/src/ops/function.rs:250:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  140: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/alloc/src/boxed.rs:1966:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  141: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/alloc/src/boxed.rs:1966:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  142: std::sys::pal::unix::thread::Thread::new::thread_start
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/sys/pal/unix/thread.rs:109:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321  143: __pthread_cond_wait
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321 
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321 query stacktrace:
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321    0: infer_scope_types(Id(800)) -> (R241, Durability::LOW)
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at crates/ty_python_semantic/src/types/infer.rs:135
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321    1: check_types(Id(0)) -> (R240, Durability::LOW)
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321              at crates/ty_python_semantic/src/types.rs:88
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1321 

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='is_subtype_of.md - Subtype relation - Callable - Class literals - Classes with `__init… (5c313ee5578fff58)'
MDTEST_TEST_FILTER='is_subtype_of.md - Subtype relation - Callable - Class literals - Classes with `__init… (5c313ee5578fff58)' cargo test -p ty_python_semantic --test mdtest -- mdtest__type_properties_is_subtype_of

is_subtype_of.md - Subtype relation - Callable - Bound methods (13cdf4c30a49b158)

  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403 panicked at crates/ty_python_semantic/src/types/signatures.rs:555:52
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403 called `Option::unwrap()` on a `None` value
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403    0: std::backtrace_rs::backtrace::libunwind::trace
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/../../backtrace/src/backtrace/libunwind.rs:117:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403    1: std::backtrace_rs::backtrace::trace_unsynchronized
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/../../backtrace/src/backtrace/mod.rs:66:14
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403    2: std::backtrace::Backtrace::create
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/backtrace.rs:331:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403    3: ruff_db::panic::install_hook::{{closure}}::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/dev/ruff/crates/ruff_db/src/panic.rs:91:34
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403    4: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/alloc/src/boxed.rs:1980:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403    5: std::panicking::rust_panic_with_hook
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/panicking.rs:841:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403    6: std::panicking::begin_panic_handler::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/panicking.rs:699:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403    7: std::sys::backtrace::__rust_end_short_backtrace
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/sys/backtrace.rs:168:18
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403    8: __rustc::rust_begin_unwind
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/panicking.rs:697:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403    9: core::panicking::panic_fmt
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/core/src/panicking.rs:75:14
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   10: core::panicking::panic
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/core/src/panicking.rs:145:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   11: core::option::unwrap_failed
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/core/src/option.rs:2015:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   12: core::option::Option<T>::unwrap
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:978:21
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   13: ty_python_semantic::types::signatures::Signature::is_subtype_of::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/signatures.rs:555:46
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   14: ty_python_semantic::types::signatures::Signature::is_assignable_to_impl
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/signatures.rs:708:33
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   15: ty_python_semantic::types::signatures::Signature::is_subtype_of
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/signatures.rs:553:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   16: ty_python_semantic::types::signatures::CallableSignature::is_subtype_of::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/signatures.rs:120:48
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   17: ty_python_semantic::types::signatures::CallableSignature::has_relation_to_impl
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/signatures.rs:150:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   18: ty_python_semantic::types::signatures::CallableSignature::is_subtype_of
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/signatures.rs:117:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   19: ty_python_semantic::types::signatures::CallableSignature::has_relation_to
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/signatures.rs:108:44
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   20: ty_python_semantic::types::CallableType::has_relation_to
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types.rs:7085:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   21: ty_python_semantic::types::Type::has_relation_to
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types.rs:1289:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   22: ty_python_semantic::types::Type::has_relation_to
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types.rs:1262:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   23: ty_python_semantic::types::Type::is_subtype_of
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types.rs:1057:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   24: ty_python_semantic::types::call::bind::Bindings::evaluate_known_cases
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/call/bind.rs:568:37
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   25: ty_python_semantic::types::call::bind::Bindings::check_types
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/call/bind.rs:126:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   26: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_call_expression
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/infer.rs:5185:15
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   27: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/infer.rs:4498:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   28: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_expression
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/infer.rs:4443:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   29: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_argument_type
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/infer.rs:4427:50
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   30: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_argument_types::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/infer.rs:4412:26
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   31: ty_python_semantic::types::call::arguments::CallArgumentTypes::new::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/call/arguments.rs:89:36
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   32: core::iter::adapters::map::map_fold::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/map.rs:88:28
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   33: <core::iter::adapters::enumerate::Enumerate<I> as core::iter::traits::iterator::Iterator>::fold::enumerate::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/enumerate.rs:107:27
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   34: core::iter::adapters::copied::copy_fold::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/copied.rs:30:22
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   35: <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/slice/iter/macros.rs:255:27
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   36: <core::iter::adapters::copied::Copied<I> as core::iter::traits::iterator::Iterator>::fold
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/copied.rs:75:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   37: <core::iter::adapters::enumerate::Enumerate<I> as core::iter::traits::iterator::Iterator>::fold
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/enumerate.rs:113:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   38: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/map.rs:128:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   39: core::iter::traits::iterator::Iterator::for_each
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:800:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   40: alloc::vec::Vec<T,A>::extend_trusted
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3579:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   41: <alloc::vec::Vec<T,A> as alloc::vec::spec_extend::SpecExtend<T,I>>::spec_extend
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_extend.rs:29:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   42: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter_nested.rs:62:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   43: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter::SpecFromIter<T,I>>::from_iter
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter.rs:34:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   44: <alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3438:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   45: core::iter::traits::iterator::Iterator::collect
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:1985:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   46: ty_python_semantic::types::call::arguments::CallArgumentTypes::new
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/call/arguments.rs:86:21
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   47: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_argument_types
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/infer.rs:4401:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   48: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_call_expression
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/infer.rs:5183:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   49: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/infer.rs:4498:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   50: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_expression
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/infer.rs:4443:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   51: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_statement
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/infer.rs:1986:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   52: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_body
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/infer.rs:1977:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   53: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_module
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/infer.rs:1734:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   54: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_region_scope
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/infer.rs:780:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   55: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_region
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/infer.rs:769:46
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   56: ty_python_semantic::types::infer::TypeInferenceBuilder::finish
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/infer.rs:8189:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   57: <ty_python_semantic::types::infer::infer_scope_types::Configuration_ as salsa::function::Configuration>::execute::inner_
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types/infer.rs:146:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   58: <ty_python_semantic::types::infer::infer_scope_types::Configuration_ as salsa::function::Configuration>::execute
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/components/salsa-macro-rules/src/setup_tracked_fn.rs:204:21
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   59: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_query
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/execute.rs:283:25
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   60: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_maybe_iterate
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/execute.rs:139:50
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   61: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/execute.rs:90:48
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   62: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:255:20
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   63: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold_with_retry
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:95:20
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   64: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:52:21
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   65: core::option::Option<T>::or_else
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   66: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:49:33
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   67: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:21:20
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   68: ty_python_semantic::types::infer::infer_scope_types::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/components/salsa-macro-rules/src/setup_tracked_fn.rs:362:25
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   69: salsa::attach::Attached::attach
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/attach.rs:79:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   70: salsa::attach::attach::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/attach.rs:103:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   71: std::thread::local::LocalKey<T>::try_with
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:311:12
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   72: std::thread::local::LocalKey<T>::with
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:275:15
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   73: salsa::attach::attach
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/attach.rs:101:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   74: ty_python_semantic::types::infer::infer_scope_types
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/components/salsa-macro-rules/src/setup_tracked_fn.rs:354:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   75: <ty_python_semantic::types::check_types::Configuration_ as salsa::function::Configuration>::execute::inner_
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./src/types.rs:98:22
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   76: <ty_python_semantic::types::check_types::Configuration_ as salsa::function::Configuration>::execute
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/components/salsa-macro-rules/src/setup_tracked_fn.rs:204:21
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   77: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_query
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/execute.rs:283:25
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   78: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/execute.rs:44:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   79: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:255:20
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   80: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold_with_retry
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:95:20
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   81: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:52:21
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   82: core::option::Option<T>::or_else
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   83: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:49:33
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   84: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/function/fetch.rs:21:20
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   85: ty_python_semantic::types::check_types::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/components/salsa-macro-rules/src/setup_tracked_fn.rs:362:25
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   86: salsa::attach::Attached::attach
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/attach.rs:79:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   87: salsa::attach::attach::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/attach.rs:103:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   88: std::thread::local::LocalKey<T>::try_with
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:311:12
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   89: std::thread::local::LocalKey<T>::with
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:275:15
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   90: salsa::attach::attach
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/src/attach.rs:101:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   91: ty_python_semantic::types::check_types
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.cargo/git/checkouts/salsa-ef8eba5d25c15f8b/0f6d406/components/salsa-macro-rules/src/setup_tracked_fn.rs:354:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   92: ty_test::run_test::{{closure}}::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/dev/ruff/crates/ty_test/src/lib.rs:312:58
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   93: std::panicking::try::do_call
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:589:40
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   94: ___rust_try
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   95: std::panicking::try
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:552:19
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   96: std::panic::catch_unwind
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panic.rs:359:14
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   97: ruff_db::panic::catch_unwind
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/dev/ruff/crates/ruff_db/src/panic.rs:122:18
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   98: ty_test::run_test::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/dev/ruff/crates/ty_test/src/lib.rs:312:42
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403   99: core::ops::function::impls::<impl core::ops::function::FnMut<A> for &mut F>::call_mut
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:294:13
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  100: core::iter::traits::iterator::Iterator::find_map::check::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2873:32
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  101: <alloc::vec::into_iter::IntoIter<T,A> as core::iter::traits::iterator::Iterator>::try_fold
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/into_iter.rs:346:25
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  102: core::iter::traits::iterator::Iterator::find_map
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2879:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  103: <core::iter::adapters::filter_map::FilterMap<I,F> as core::iter::traits::iterator::Iterator>::next
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/adapters/filter_map.rs:64:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  104: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter_nested.rs:25:32
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  105: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter{{reify.shim}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter_nested.rs:19:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  106: alloc::vec::in_place_collect::<impl alloc::vec::spec_from_iter::SpecFromIter<T,I> for alloc::vec::Vec<T>>::from_iter
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/in_place_collect.rs:246:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  107: <alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3438:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  108: core::iter::traits::iterator::Iterator::collect
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:1985:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  109: ty_test::run_test
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/dev/ruff/crates/ty_test/src/lib.rs:294:30
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  110: ty_test::run
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/dev/ruff/crates/ty_test/src/lib.rs:72:32
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  111: mdtest::mdtest
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./tests/mdtest.rs:28:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  112: mdtest::mdtest__type_properties_is_subtype_of
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./tests/mdtest.rs:6:1
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  113: mdtest::mdtest__type_properties_is_subtype_of::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at ./tests/mdtest.rs:9:3
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  114: core::ops::function::FnOnce::call_once
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  115: core::ops::function::FnOnce::call_once
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/core/src/ops/function.rs:250:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  116: test::__rust_begin_short_backtrace
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/test/src/lib.rs:638:18
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  117: test::run_test_in_process::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/test/src/lib.rs:661:60
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  118: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/core/src/panic/unwind_safe.rs:272:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  119: std::panicking::try::do_call
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/panicking.rs:589:40
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  120: std::panicking::try
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/panicking.rs:552:19
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  121: std::panic::catch_unwind
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/panic.rs:359:14
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  122: test::run_test_in_process
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/test/src/lib.rs:661:27
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  123: test::run_test::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/test/src/lib.rs:582:43
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  124: test::run_test::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/test/src/lib.rs:612:41
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  125: std::sys::backtrace::__rust_begin_short_backtrace
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/sys/backtrace.rs:152:18
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  126: std::thread::Builder::spawn_unchecked_::{{closure}}::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/thread/mod.rs:559:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  127: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/core/src/panic/unwind_safe.rs:272:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  128: std::panicking::try::do_call
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/panicking.rs:589:40
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  129: std::panicking::try
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/panicking.rs:552:19
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  130: std::panic::catch_unwind
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/panic.rs:359:14
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  131: std::thread::Builder::spawn_unchecked_::{{closure}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/thread/mod.rs:557:30
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  132: core::ops::function::FnOnce::call_once{{vtable.shim}}
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/core/src/ops/function.rs:250:5
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  133: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/alloc/src/boxed.rs:1966:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  134: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/alloc/src/boxed.rs:1966:9
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  135: std::sys::pal::unix::thread::Thread::new::thread_start
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/sys/pal/unix/thread.rs:109:17
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403  136: __pthread_cond_wait
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403 
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403 query stacktrace:
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403    0: infer_scope_types(Id(800)) -> (R253, Durability::LOW)
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at crates/ty_python_semantic/src/types/infer.rs:135
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403    1: check_types(Id(0)) -> (R252, Durability::LOW)
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403              at crates/ty_python_semantic/src/types.rs:88
  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1403 

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='is_subtype_of.md - Subtype relation - Callable - Bound methods (13cdf4c30a49b158)'
MDTEST_TEST_FILTER='is_subtype_of.md - Subtype relation - Callable - Bound methods (13cdf4c30a49b158)' cargo test -p ty_python_semantic --test mdtest -- mdtest__type_properties_is_subtype_of

--------------------------------------------------


thread 'mdtest__type_properties_is_subtype_of' panicked at crates/ty_test/src/lib.rs:120:5:
Some tests failed.
stack backtrace:
   0: __rustc::rust_begin_unwind
             at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/std/src/panicking.rs:697:5
   1: core::panicking::panic_fmt
             at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/core/src/panicking.rs:75:14
   2: ty_test::run
             at /Users/alexw/dev/ruff/crates/ty_test/src/lib.rs:120:5
   3: mdtest::mdtest
             at ./tests/mdtest.rs:28:5
   4: mdtest::mdtest__type_properties_is_subtype_of
             at ./tests/mdtest.rs:6:1
   5: mdtest::mdtest__type_properties_is_subtype_of::{{closure}}
             at ./tests/mdtest.rs:9:3
   6: core::ops::function::FnOnce::call_once
             at /Users/alexw/.rustup/toolchains/1.87-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
   7: core::ops::function::FnOnce::call_once
             at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library/core/src/ops/function.rs:250:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.


failures:
    mdtest__type_properties_is_subtype_of
```

</details>

---

_@AlexWaygood reviewed on 2025-06-09 11:01_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6831 on 2025-06-09 11:01_

Ugh, the `many_tuple_assignments` benchmark is now actually showing a [1% regression](https://codspeed.io/astral-sh/ruff/branches/alex%2Foptimize-subtyping) after making this change :-(

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:558 on 2025-06-09 11:03_

that's the results of running `RUST_BACKTRACE=1 cargo test -p ty_python_semantic --test mdtest` after applying this patch to my branch:

```diff
--- a/crates/ty_python_semantic/src/types/signatures.rs
+++ b/crates/ty_python_semantic/src/types/signatures.rs
@@ -545,15 +545,14 @@ impl<'db> Signature<'db> {
 
     /// Return `true` if a callable with signature `self` is a subtype of a callable with signature
     /// `other`.
+    ///
+    /// # Panics
+    ///
+    /// Panics if `self` or `other` is not a fully static signature.
     pub(crate) fn is_subtype_of(&self, db: &'db dyn Db, other: &Signature<'db>) -> bool {
         self.is_assignable_to_impl(other, |type1, type2| {
-            let Some(type1) = type1 else {
-                return false;
-            };
-            let Some(type2) = type2 else {
-                return false;
-            };
-            type1.is_subtype_of(db, type2)
+            // SAFETY: Subtype relation is only checked for fully static types.
+            type1.unwrap().is_subtype_of(db, type2.unwrap())
         })
     }
```

---

_@AlexWaygood reviewed on 2025-06-09 11:03_

---

_@dhruvmanila reviewed on 2025-06-09 11:19_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:558 on 2025-06-09 11:19_

(Heading to the gym, I'll look at this once I'm back, thank you for providing the details!)

---

_@dhruvmanila reviewed on 2025-06-10 02:28_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:6952 on 2025-06-10 02:28_

> Hmm, I suppose I could turn these `if` checks into `debug_assert!`s that check we're not passing `TypeRelation::Subtyping` into a `has_relation_to()` call for non-fully-static-types?

This might be a good starting point.

> We do have places around the codebase that call `ClassType::is_subclass_of`... but that's a bit of an "odd one out", because `ClassType`s don't really have assignability or subtyping relations to each other.

Yes, but `ClassType` isn't part of the `Type` enum. Maybe it could also have the same `debug_assert` given that it's only called as part of another subtyping check which in turn would be called via `Type::has_relation_to` if I'm understanding this correctly?

I'll leave this up to you as you've the most knowledge in this area.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:558 on 2025-06-10 02:43_

I think I know why this might be happening -- the `CallableSignature::has_relation_to` directly calls the `Signature::is_subtype_of` without checking whether it's fully static or not. The reason we need to check whether a signature is fully static or not is because `FunctionLiteral`s are fully static but subtyping of `FunctionLiteral` needs to check whether the signature itself is also fully static. You can see this happening in `FunctionType::is_subtype_of` and `CallableSignature::is_equivalent_to`.

For more context, the specific example that was failing was:

```py
from typing import Callable
from ty_extensions import TypeOf, static_assert, is_subtype_of

class A:
    def f(self, a: int) -> int:
        return a

    @classmethod
    def g(cls, a: int) -> int:
        return a

a = A()

# This seems to be causing the panic
static_assert(is_subtype_of(TypeOf[A.f], Callable[[A, int], int]))
```

Because, the `A.f` returns a `FunctionLiteral` where `self` is unannotated. This is not an intended behavior and there's a `TODO` in the test case to resolve this: https://github.com/astral-sh/ruff/blob/df64227aa27ce395118485cad3df1eab69d2aec8/crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md?plain=1#L1423-L1425

For now, you can get away by adding the `is_fully_static` checks. I want to create a follow-up PR (as promised :)) to simplify the relation checks on the signature side.

```diff
diff --git a/crates/ty_python_semantic/src/types/signatures.rs b/crates/ty_python_semantic/src/types/signatures.rs
index da0d1e1b91..d14dbdaf5c 100644
--- a/crates/ty_python_semantic/src/types/signatures.rs
+++ b/crates/ty_python_semantic/src/types/signatures.rs
@@ -105,7 +105,12 @@ impl<'db> CallableSignature<'db> {
         relation: &TypeRelation,
     ) -> bool {
         match relation {
-            TypeRelation::Subtyping(..) => self.is_subtype_of(db, other),
+            TypeRelation::Subtyping(..) => {
+                if !self.is_fully_static(db) || !other.is_fully_static(db) {
+                    return false;
+                }
+                self.is_subtype_of(db, other)
+            }
             TypeRelation::Assignability => self.is_assignable_to(db, other),
         }
     }
@@ -545,15 +550,14 @@ impl<'db> Signature<'db> {
 
     /// Return `true` if a callable with signature `self` is a subtype of a callable with signature
     /// `other`.
+    ///
+    /// # Panics
+    ///
+    /// Panics if `self` or `other` is not a fully static signature.
     pub(crate) fn is_subtype_of(&self, db: &'db dyn Db, other: &Signature<'db>) -> bool {
         self.is_assignable_to_impl(other, |type1, type2| {
-            let Some(type1) = type1 else {
-                return false;
-            };
-            let Some(type2) = type2 else {
-                return false;
-            };
-            type1.is_subtype_of(db, type2)
+            // SAFETY: Subtype relation is only checked for fully static types.
+            type1.unwrap().is_subtype_of(db, type2.unwrap())
         })
     }
```

---

_@dhruvmanila reviewed on 2025-06-10 02:43_

---

_@AlexWaygood reviewed on 2025-06-10 06:42_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:558 on 2025-06-10 06:42_

Ohh of course. Thanks so much!!

---

_@MichaReiser reviewed on 2025-06-10 06:56_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:6823 on 2025-06-10 06:56_

Nit: Maybe rename to `SubtypingApplicability` to make it clear what this is about.

It's also unfortunate that this implementation detail (that we use caching) is now a pub(crate) type 

---

_@MichaReiser reviewed on 2025-06-10 06:58_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:6794 on 2025-06-10 06:58_

Hmm, what happens if `applise_to` is called multiple times but with different types. Won't we then return incorret results from `get_or_init`?

I feel like this makes this API somewhat error prone and risks introducing very subtle caching bugs. I'd prefer it if the following were somehow expressed as part of the cache type: This is a cached result for `type_1` and `type_2`.

---

_@dhruvmanila reviewed on 2025-06-10 08:13_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:6794 on 2025-06-10 08:13_

Correct me if I'm wrong @AlexWaygood, but I don't think the goal here is to cache this value based on the two types that are used for this call, the goal is to make sure that this is being called once when performing a subtyping check for any two types. The cached value would be local to that call.

For example, when two `CallableType`s are being checked for subtyping, this will first check whether both callable types are fully static. If yes, then we'll proceed with subtyping check. Now, these callable types also contains other `Type`s specifically the parameter types and return types which itself could be a union of other types. Here, we don't need to check whether these types that are contained within the outer callable type is fully static or not. They were already checked as part of checking whether the top level callable type is fully static or not. This cached value represents that part.

Does that make sense? There's some more context on my message on Discord: https://discord.com/channels/1039017663004942429/1228460843033821285/1380807857594699806

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6794 on 2025-06-10 08:25_

That's exactly correct. Invalidating the cache if a different pair of types was passed in would defeat the point of having the cache altogether. The premise of this PR is that no fully static type can contain a non-fully-static type, so it's redundant to check whether an inner type is fully static if you've already checked that an outer type which contains that type is fully static

---

_@AlexWaygood reviewed on 2025-06-10 08:25_

---

_@MichaReiser reviewed on 2025-06-10 09:01_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:6794 on 2025-06-10 09:01_

> Correct me if I'm wrong @AlexWaygood, but I don't think the goal here is to cache this value based on the two types that are used for this call, the goal is to make sure that this is being called once when performing a subtyping check for any two types. The cached value would be local to that call.

I think I understand what you're saying but it seems that there's nothing enforcing the invariant that this indeed remains local to that call.

---

_@dhruvmanila reviewed on 2025-06-10 11:08_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:6794 on 2025-06-10 11:08_

Yeah, I'm not sure how best to enforce that at the type level

---

_@MichaReiser reviewed on 2025-06-10 11:14_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:6794 on 2025-06-10 11:14_

I think a debug assertion would be great. But it requires storing the two types in the cache. 

---

_@carljm reviewed on 2025-06-10 21:20_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:558 on 2025-06-10 21:20_

I think the conclusion of our discussion on the topic was that we should not actually treat FunctionLiteral as fully static if the signature is not fully static -- it doesn't work to have a "fully static" type where we can't determine its subtyping against another fully static type.

---

_@AlexWaygood reviewed on 2025-06-10 21:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6794 on 2025-06-10 21:21_

Yeah, I'll add some more debug assertions to defend the invariants here.

---

_Comment by @carljm on 2025-06-10 21:22_

FWIW I think in the short to medium term we should be aiming to eliminate `is_fully_static` entirely, in favor of using top/bottom materializations for subtype checks involving non-fully-static types. (And @dhruvmanila 's PR adding those materializations moves us a lot closer to making that possible.) So I don't think we should devote a lot of time to optimizations involving `is_fully_static`, especially if they don't show up noticeably in real-world benchmarks.

---

_Comment by @AlexWaygood on 2025-06-12 10:52_

Well, I'm glad I experimented with it, but the lacklustre benchmark results and Carl's comment indicate I probably shouldn't spend any more time on this right now.

---

_Closed by @AlexWaygood on 2025-06-12 10:52_

---

_Branch deleted on 2025-06-12 10:52_

---
