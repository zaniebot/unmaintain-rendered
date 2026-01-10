```yaml
number: 17792
title: "[red-knot] Panic: \"expression should belong to this TypeInference region\""
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - ty
assignees: []
created_at: 2025-05-02T12:06:16Z
updated_at: 2025-05-05T11:52:10Z
url: https://github.com/astral-sh/ruff/issues/17792
synced_at: 2026-01-10T11:09:58Z
```

# [red-knot] Panic: "expression should belong to this TypeInference region"

---

_Issue opened by @sharkdp on 2025-05-02 12:06_

After fixing one panic on scipy, there is another panic remaining. I narrowed it down to this snippet:

```py
class C:
    def __init__(self, cls):
        if isinstance(cls, C):
            cls, _ = cls.args

        self.args = (cls, 1)
```

<details>

<summary>click for full backtrace
<pre>
error: panic: Panicked at crates/red_knot_python_semantic/src/types/infer.rs:3971:14 when checking `/home/shark/_interface.py`: `expression should belong to this TypeInference region and TypeInferenceBuilder should have inferred a type for it`</pre>
</summary>

```
error: panic: Panicked at crates/red_knot_python_semantic/src/types/infer.rs:3971:14 when checking `/home/shark/_interface.py`: `expression should belong to this TypeInference region and TypeInferenceBuilder should have inferred a type for it`
info: This indicates a bug in Red Knot.
info: If you could open an issue at https://github.com/astral-sh/ruff/issues/new?title=%5Bred-knot%5D:%20panic we'd be very appreciative!
info: Platform: linux x86_64
info: Args: ["/home/shark/.cargo-target/debug/red_knot", "check", "/home/shark/_interface.py"]
info: Backtrace:
   0: ruff_db::panic::install_hook::{{closure}}::{{closure}}
             at ./crates/ruff_db/src/panic.rs:81:34
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/alloc/src/boxed.rs:1990:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panicking.rs:839:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panicking.rs:704:13
   4: std::sys::backtrace::__rust_end_short_backtrace
             at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/sys/backtrace.rs:168:18
   5: rust_begin_unwind
             at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/panicking.rs:695:5
   6: core::panicking::panic_fmt
             at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/core/src/panicking.rs:75:14
   7: core::panicking::panic_display
             at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/core/src/panicking.rs:261:5
   8: core::option::expect_failed
             at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/core/src/option.rs:2024:5
   9: core::option::Option<T>::expect
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/option.rs:933:21
  10: red_knot_python_semantic::types::infer::TypeInference::expression_type
             at ./crates/red_knot_python_semantic/src/types/infer.rs:401:9
  11: red_knot_python_semantic::types::infer::TypeInferenceBuilder::expression_type
             at ./crates/red_knot_python_semantic/src/types/infer.rs:661:9
  12: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_standalone_expression
             at ./crates/red_knot_python_semantic/src/types/infer.rs:3971:9
  13: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_assignment_definition
             at ./crates/red_knot_python_semantic/src/types/infer.rs:3175:24
  14: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region_definition
             at ./crates/red_knot_python_semantic/src/types/infer.rs:1222:17
  15: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region
             at ./crates/red_knot_python_semantic/src/types/infer.rs:693:56
  16: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
             at ./crates/red_knot_python_semantic/src/types/infer.rs:7031:9
  17: <red_knot_python_semantic::types::infer::infer_definition_types::Configuration_ as salsa::function::Configuration>::execute::inner_
             at ./crates/red_knot_python_semantic/src/types/infer.rs:159:5
  18: <red_knot_python_semantic::types::infer::infer_definition_types::Configuration_ as salsa::function::Configuration>::execute
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:207:21
  19: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_query
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:266:25
  20: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_maybe_iterate
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:146:17
  21: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:89:48
  22: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:197:20
  23: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:46:29
  24: core::option::Option<T>::or_else
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
  25: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:44:33
  26: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:17:20
  27: red_knot_python_semantic::types::infer::infer_definition_types::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:368:25
  28: salsa::attach::Attached::attach
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:73:9
  29: salsa::attach::attach::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:97:13
  30: std::thread::local::LocalKey<T>::try_with
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:310:12
  31: std::thread::local::LocalKey<T>::with
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:274:15
  32: salsa::attach::attach
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:95:5
  33: red_knot_python_semantic::types::infer::infer_definition_types
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:360:13
  34: red_knot_python_semantic::types::binding_type
             at ./crates/red_knot_python_semantic/src/types.rs:110:21
  35: red_knot_python_semantic::symbol::symbol_from_bindings_impl::{{closure}}
             at ./crates/red_knot_python_semantic/src/symbol.rs:798:30
  36: core::ops::function::impls::<impl core::ops::function::FnMut<A> for &mut F>::call_mut
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:294:13
  37: core::iter::traits::iterator::Iterator::find_map::check::{{closure}}
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2859:32
  38: core::iter::traits::iterator::Iterator::try_fold
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2370:21
  39: <core::iter::adapters::peekable::Peekable<I> as core::iter::traits::iterator::Iterator>::try_fold
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/adapters/peekable.rs:103:9
  40: core::iter::traits::iterator::Iterator::find_map
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2865:9
  41: <core::iter::adapters::filter_map::FilterMap<I,F> as core::iter::traits::iterator::Iterator>::next
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/adapters/filter_map.rs:64:9
  42: red_knot_python_semantic::symbol::symbol_from_bindings_impl
             at ./crates/red_knot_python_semantic/src/symbol.rs:824:31
  43: red_knot_python_semantic::symbol::symbol_from_bindings
             at ./crates/red_knot_python_semantic/src/symbol.rs:392:5
  44: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_name_load
             at ./crates/red_knot_python_semantic/src/types/infer.rs:5102:13
  45: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_name_expression
             at ./crates/red_knot_python_semantic/src/types/infer.rs:5270:34
  46: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
             at ./crates/red_knot_python_semantic/src/types/infer.rs:3993:38
  47: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_expression
             at ./crates/red_knot_python_semantic/src/types/infer.rs:3964:9
  48: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_tuple_expression::{{closure}}
             at ./crates/red_knot_python_semantic/src/types/infer.rs:4141:59
  49: core::iter::adapters::map::map_fold::{{closure}}
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/adapters/map.rs:88:28
  50: <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/slice/iter/macros.rs:232:27
  51: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/adapters/map.rs:128:9
  52: core::iter::traits::iterator::Iterator::for_each
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:800:9
  53: alloc::vec::Vec<T,A>::extend_trusted
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3565:17
  54: <alloc::vec::Vec<T,A> as alloc::vec::spec_extend::SpecExtend<T,I>>::spec_extend
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/alloc/src/vec/spec_extend.rs:29:9
  55: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter_nested.rs:62:9
  56: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter::SpecFromIter<T,I>>::from_iter
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter.rs:34:9
  57: <alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3424:9
  58: core::iter::traits::iterator::Iterator::collect
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:1971:9
  59: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_tuple_expression
             at ./crates/red_knot_python_semantic/src/types/infer.rs:4141:37
  60: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
             at ./crates/red_knot_python_semantic/src/types/infer.rs:3985:40
  61: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region_expression
             at ./crates/red_knot_python_semantic/src/types/infer.rs:1293:17
  62: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region
             at ./crates/red_knot_python_semantic/src/types/infer.rs:695:56
  63: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
             at ./crates/red_knot_python_semantic/src/types/infer.rs:7031:9
  64: <red_knot_python_semantic::types::infer::infer_expression_types::Configuration_ as salsa::function::Configuration>::execute::inner_
             at ./crates/red_knot_python_semantic/src/types/infer.rs:234:5
  65: <red_knot_python_semantic::types::infer::infer_expression_types::Configuration_ as salsa::function::Configuration>::execute
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:207:21
  66: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_query
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:266:25
  67: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_maybe_iterate
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:146:17
  68: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:89:48
  69: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:197:20
  70: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:46:29
  71: core::option::Option<T>::or_else
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
  72: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:44:33
  73: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:17:20
  74: red_knot_python_semantic::types::infer::infer_expression_types::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:368:25
  75: salsa::attach::Attached::attach
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:73:9
  76: salsa::attach::attach::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:97:13
  77: std::thread::local::LocalKey<T>::try_with
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:310:12
  78: std::thread::local::LocalKey<T>::with
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:274:15
  79: salsa::attach::attach
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:95:5
  80: red_knot_python_semantic::types::infer::infer_expression_types
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:360:13
  81: red_knot_python_semantic::types::infer::infer_same_file_expression_type
             at ./crates/red_knot_python_semantic/src/types/infer.rs:262:21
  82: <red_knot_python_semantic::types::infer::infer_expression_type::Configuration_ as salsa::function::Configuration>::execute::inner_
             at ./crates/red_knot_python_semantic/src/types/infer.rs:280:5
  83: <red_knot_python_semantic::types::infer::infer_expression_type::Configuration_ as salsa::function::Configuration>::execute
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:207:21
  84: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_query
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:266:25
  85: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_maybe_iterate
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:146:17
  86: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:89:48
  87: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:197:20
  88: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:46:29
  89: core::option::Option<T>::or_else
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
  90: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:44:33
  91: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:17:20
  92: red_knot_python_semantic::types::infer::infer_expression_type::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:368:25
  93: salsa::attach::Attached::attach
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:73:9
  94: salsa::attach::attach::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:97:13
  95: std::thread::local::LocalKey<T>::try_with
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:310:12
  96: std::thread::local::LocalKey<T>::with
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:274:15
  97: salsa::attach::attach
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:95:5
  98: red_knot_python_semantic::types::infer::infer_expression_type
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:360:13
  99: red_knot_python_semantic::types::class::ClassLiteral::implicit_instance_attribute
             at ./crates/red_knot_python_semantic/src/types/class.rs:1451:37
 100: red_knot_python_semantic::types::class::ClassLiteral::own_instance_member
             at ./crates/red_knot_python_semantic/src/types/class.rs:1663:13
 101: red_knot_python_semantic::types::class::ClassType::own_instance_member
             at ./crates/red_knot_python_semantic/src/types/class.rs:410:9
 102: red_knot_python_semantic::types::class::ClassLiteral::instance_member
             at ./crates/red_knot_python_semantic/src/types/class.rs:1285:25
 103: red_knot_python_semantic::types::class::ClassType::instance_member
             at ./crates/red_knot_python_semantic/src/types/class.rs:401:9
 104: red_knot_python_semantic::types::Type::instance_member
             at ./crates/red_knot_python_semantic/src/types.rs:2528:48
 105: <red_knot_python_semantic::types::Type as red_knot_python_semantic::types::Type::member_lookup_with_policy::InnerTrait_>::member_lookup_with_policy_
             at ./crates/red_knot_python_semantic/src/types.rs:3035:32
 106: <red_knot_python_semantic::types::Type::member_lookup_with_policy::member_lookup_with_policy_::Configuration_ as salsa::function::Configuration>::execute::inner_
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_method_body.rs:34:17
 107: <red_knot_python_semantic::types::Type::member_lookup_with_policy::member_lookup_with_policy_::Configuration_ as salsa::function::Configuration>::execute
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:207:21
 108: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_query
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:266:25
 109: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_maybe_iterate
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:146:17
 110: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:89:48
 111: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:197:20
 112: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:46:29
 113: core::option::Option<T>::or_else
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
 114: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:44:33
 115: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:17:20
 116: red_knot_python_semantic::types::Type::member_lookup_with_policy::member_lookup_with_policy_::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:365:29
 117: salsa::attach::Attached::attach
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:73:9
 118: salsa::attach::attach::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:97:13
 119: std::thread::local::LocalKey<T>::try_with
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:310:12
 120: std::thread::local::LocalKey<T>::with
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:274:15
 121: salsa::attach::attach
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:95:5
 122: red_knot_python_semantic::types::Type::member_lookup_with_policy::member_lookup_with_policy_
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:360:13
 123: red_knot_python_semantic::types::Type::member_lookup_with_policy
             at ./crates/red_knot_python_semantic/src/types.rs:536:1
 124: <red_knot_python_semantic::types::Type as red_knot_python_semantic::types::Type::member_lookup_with_policy::InnerTrait_>::member_lookup_with_policy_::{{closure}}
             at ./crates/red_knot_python_semantic/src/types.rs:2898:21
 125: red_knot_python_semantic::types::IntersectionType::map_with_boundness
             at ./crates/red_knot_python_semantic/src/types.rs:7716:29
 126: <red_knot_python_semantic::types::Type as red_knot_python_semantic::types::Type::member_lookup_with_policy::InnerTrait_>::member_lookup_with_policy_
             at ./crates/red_knot_python_semantic/src/types.rs:2896:49
 127: <red_knot_python_semantic::types::Type::member_lookup_with_policy::member_lookup_with_policy_::Configuration_ as salsa::function::Configuration>::execute::inner_
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_method_body.rs:34:17
 128: <red_knot_python_semantic::types::Type::member_lookup_with_policy::member_lookup_with_policy_::Configuration_ as salsa::function::Configuration>::execute
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:207:21
 129: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_query
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:266:25
 130: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_maybe_iterate
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:146:17
 131: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:89:48
 132: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:197:20
 133: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:46:29
 134: core::option::Option<T>::or_else
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
 135: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:44:33
 136: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:17:20
 137: red_knot_python_semantic::types::Type::member_lookup_with_policy::member_lookup_with_policy_::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:365:29
 138: salsa::attach::Attached::attach
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:73:9
 139: salsa::attach::attach::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:97:13
 140: std::thread::local::LocalKey<T>::try_with
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:310:12
 141: std::thread::local::LocalKey<T>::with
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:274:15
 142: salsa::attach::attach
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:95:5
 143: red_knot_python_semantic::types::Type::member_lookup_with_policy::member_lookup_with_policy_
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:360:13
 144: red_knot_python_semantic::types::Type::member_lookup_with_policy
             at ./crates/red_knot_python_semantic/src/types.rs:536:1
 145: red_knot_python_semantic::types::Type::member
             at ./crates/red_knot_python_semantic/src/types.rs:2869:9
 146: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_attribute_load
             at ./crates/red_knot_python_semantic/src/types/infer.rs:5288:9
 147: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_attribute_expression
             at ./crates/red_knot_python_semantic/src/types/infer.rs:5361:34
 148: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
             at ./crates/red_knot_python_semantic/src/types/infer.rs:3994:48
 149: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region_expression
             at ./crates/red_knot_python_semantic/src/types/infer.rs:1293:17
 150: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region
             at ./crates/red_knot_python_semantic/src/types/infer.rs:695:56
 151: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
             at ./crates/red_knot_python_semantic/src/types/infer.rs:7031:9
 152: <red_knot_python_semantic::types::infer::infer_expression_types::Configuration_ as salsa::function::Configuration>::execute::inner_
             at ./crates/red_knot_python_semantic/src/types/infer.rs:234:5
 153: <red_knot_python_semantic::types::infer::infer_expression_types::Configuration_ as salsa::function::Configuration>::execute
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:207:21
 154: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_query
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:266:25
 155: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_maybe_iterate
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:146:17
 156: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:89:48
 157: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:197:20
 158: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:46:29
 159: core::option::Option<T>::or_else
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
 160: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:44:33
 161: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:17:20
 162: red_knot_python_semantic::types::infer::infer_expression_types::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:368:25
 163: salsa::attach::Attached::attach
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:73:9
 164: salsa::attach::attach::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:97:13
 165: std::thread::local::LocalKey<T>::try_with
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:310:12
 166: std::thread::local::LocalKey<T>::with
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:274:15
 167: salsa::attach::attach
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:95:5
 168: red_knot_python_semantic::types::infer::infer_expression_types
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:360:13
 169: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_standalone_expression
             at ./crates/red_knot_python_semantic/src/types/infer.rs:3969:21
 170: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_assignment_statement::{{closure}}
             at ./crates/red_knot_python_semantic/src/types/infer.rs:2661:17
 171: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_target
             at ./crates/red_knot_python_semantic/src/types/infer.rs:2681:23
 172: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_assignment_statement
             at ./crates/red_knot_python_semantic/src/types/infer.rs:2660:13
 173: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_statement
             at ./crates/red_knot_python_semantic/src/types/infer.rs:1671:42
 174: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_body
             at ./crates/red_knot_python_semantic/src/types/infer.rs:1656:13
 175: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_if_statement
             at ./crates/red_knot_python_semantic/src/types/infer.rs:2177:9
 176: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_statement
             at ./crates/red_knot_python_semantic/src/types/infer.rs:1667:44
 177: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_body
             at ./crates/red_knot_python_semantic/src/types/infer.rs:1656:13
 178: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_function_body
             at ./crates/red_knot_python_semantic/src/types/infer.rs:1582:9
 179: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region_scope
             at ./crates/red_knot_python_semantic/src/types/infer.rs:706:54
 180: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region
             at ./crates/red_knot_python_semantic/src/types/infer.rs:692:46
 181: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
             at ./crates/red_knot_python_semantic/src/types/infer.rs:7031:9
 182: <red_knot_python_semantic::types::infer::infer_scope_types::Configuration_ as salsa::function::Configuration>::execute::inner_
             at ./crates/red_knot_python_semantic/src/types/infer.rs:126:5
 183: <red_knot_python_semantic::types::infer::infer_scope_types::Configuration_ as salsa::function::Configuration>::execute
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:207:21
 184: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_query
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:266:25
 185: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_maybe_iterate
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:146:17
 186: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:89:48
 187: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:197:20
 188: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:46:29
 189: core::option::Option<T>::or_else
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
 190: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:44:33
 191: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:17:20
 192: red_knot_python_semantic::types::infer::infer_scope_types::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:368:25
 193: salsa::attach::Attached::attach
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:73:9
 194: salsa::attach::attach::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:97:13
 195: std::thread::local::LocalKey<T>::try_with
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:310:12
 196: std::thread::local::LocalKey<T>::with
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:274:15
 197: salsa::attach::attach
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:95:5
 198: red_knot_python_semantic::types::infer::infer_scope_types
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:360:13
 199: <red_knot_python_semantic::types::check_types::Configuration_ as salsa::function::Configuration>::execute::inner_
             at ./crates/red_knot_python_semantic/src/types.rs:92:22
 200: <red_knot_python_semantic::types::check_types::Configuration_ as salsa::function::Configuration>::execute
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:207:21
 201: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_query
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:266:25
 202: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:46:17
 203: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:197:20
 204: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:46:29
 205: core::option::Option<T>::or_else
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/option.rs:1552:21
 206: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:44:33
 207: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:17:20
 208: red_knot_python_semantic::types::check_types::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:368:25
 209: salsa::attach::Attached::attach
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:73:9
 210: salsa::attach::attach::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:97:13
 211: std::thread::local::LocalKey<T>::try_with
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:310:12
 212: std::thread::local::LocalKey<T>::with
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:274:15
 213: salsa::attach::attach
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:95:5
 214: red_knot_python_semantic::types::check_types
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:360:13
 215: red_knot_project::check_file_impl::{{closure}}
             at ./crates/red_knot_project/src/lib.rs:479:37
 216: std::panicking::try::do_call
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:587:40
 217: __rust_try
 218: std::panicking::try
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:550:19
 219: std::panic::catch_unwind
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:358:14
 220: salsa::cancelled::Cancelled::catch
             at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/cancelled.rs:34:15
 221: red_knot_project::catch::{{closure}}
             at ./crates/red_knot_project/src/lib.rs:583:9
 222: std::panicking::try::do_call
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:587:40
 223: __rust_try
 224: std::panicking::try
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:550:19
 225: std::panic::catch_unwind
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:358:14
 226: ruff_db::panic::catch_unwind
             at ./crates/ruff_db/src/panic.rs:112:18
 227: red_knot_project::catch
             at ./crates/red_knot_project/src/lib.rs:581:11
 228: red_knot_project::check_file_impl
             at ./crates/red_knot_project/src/lib.rs:479:15
 229: red_knot_project::Project::check::{{closure}}::{{closure}}
             at ./crates/red_knot_project/src/lib.rs:211:48
 230: rayon_core::scope::Scope::spawn::{{closure}}::{{closure}}
             at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.12.1/src/scope/mod.rs:526:57
 231: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/panic/unwind_safe.rs:272:9
 232: std::panicking::try::do_call
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:587:40
 233: __rust_try
 234: std::panicking::try
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:550:19
 235: std::panic::catch_unwind
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:358:14
 236: rayon_core::unwind::halt_unwinding
             at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.12.1/src/unwind.rs:17:5
 237: rayon_core::scope::ScopeBase::execute_job_closure
             at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.12.1/src/scope/mod.rs:689:28
 238: rayon_core::scope::ScopeBase::execute_job
             at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.12.1/src/scope/mod.rs:679:29
 239: rayon_core::scope::Scope::spawn::{{closure}}
             at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.12.1/src/scope/mod.rs:526:13
 240: <rayon_core::job::HeapJob<BODY> as rayon_core::job::Job>::execute
             at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.12.1/src/job.rs:169:9
 241: rayon_core::job::JobRef::execute
             at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.12.1/src/job.rs:64:9
 242: rayon_core::registry::WorkerThread::execute
             at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.12.1/src/registry.rs:860:9
 243: rayon_core::registry::WorkerThread::wait_until_cold
             at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.12.1/src/registry.rs:794:21
 244: rayon_core::registry::WorkerThread::wait_until
             at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.12.1/src/registry.rs:769:13
 245: rayon_core::registry::WorkerThread::wait_until_out_of_work
             at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.12.1/src/registry.rs:818:9
 246: rayon_core::registry::main_loop
             at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.12.1/src/registry.rs:923:5
 247: rayon_core::registry::ThreadBuilder::run
             at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.12.1/src/registry.rs:53:18
 248: <rayon_core::registry::DefaultSpawn as rayon_core::registry::ThreadSpawn>::spawn::{{closure}}
             at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.12.1/src/registry.rs:98:20
 249: std::sys::backtrace::__rust_begin_short_backtrace
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:152:18
 250: std::thread::Builder::spawn_unchecked_::{{closure}}::{{closure}}
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/mod.rs:559:17
 251: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/panic/unwind_safe.rs:272:9
 252: std::panicking::try::do_call
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:587:40
 253: __rust_try
 254: std::panicking::try
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:550:19
 255: std::panic::catch_unwind
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:358:14
 256: std::thread::Builder::spawn_unchecked_::{{closure}}
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/mod.rs:557:30
 257: core::ops::function::FnOnce::call_once{{vtable.shim}}
             at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
 258: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
             at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/alloc/src/boxed.rs:1976:9
 259: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
             at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/alloc/src/boxed.rs:1976:9
 260: std::sys::pal::unix::thread::Thread::new::thread_start
             at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/sys/pal/unix/thread.rs:106:17
 261: <unknown>
 262: <unknown>

info: query stacktrace:
   0: infer_definition_types(Id(1402))
             at crates/red_knot_python_semantic/src/types/infer.rs:144
             cycle heads: infer_expression_types(Id(1801)) -> 0
   1: infer_expression_types(Id(1802))
             at crates/red_knot_python_semantic/src/types/infer.rs:218
   2: infer_expression_type(Id(1802))
             at crates/red_knot_python_semantic/src/types/infer.rs:274
   3: member_lookup_with_policy_(Id(501f))
             at crates/red_knot_python_semantic/src/types.rs:536
   4: member_lookup_with_policy_(Id(501d))
             at crates/red_knot_python_semantic/src/types.rs:536
   5: infer_expression_types(Id(1801))
             at crates/red_knot_python_semantic/src/types/infer.rs:218
   6: infer_scope_types(Id(1002))
             at crates/red_knot_python_semantic/src/types/infer.rs:117
   7: check_types(Id(c00))
             at crates/red_knot_python_semantic/src/types.rs:82
```

</details>

---

_Label `bug` added by @sharkdp on 2025-05-02 12:06_

---

_Label `red-knot` added by @sharkdp on 2025-05-02 12:06_

---

_Added to milestone `Red Knot Alpha` by @sharkdp on 2025-05-02 12:07_

---

_Comment by @sharkdp on 2025-05-02 19:38_

Other minimized example that seems similar in structure, and also leads to the same panic
```py
from __future__ import annotations

from typing import Any

def f(x: Any):
    pass

C, _ = f(1)

assert issubclass(C, int)
```


---

_Comment by @dhruvmanila on 2025-05-02 20:03_

Could this be related to unpacking? I can't reproduce if I replace `C, _ = f(1)` with `C = f(1)`.

---

_Comment by @dhruvmanila on 2025-05-02 20:04_

It might not be because the example in the issue description still panics if I remove the unpacking.

---

_Comment by @sharkdp on 2025-05-03 18:36_

> Could this be related to unpacking

I think so.


> It might not be because the example in the issue description still panics if I remove the unpacking.

Yes, but with a different panic ;-)

---

_Assigned to @sharkdp by @sharkdp on 2025-05-05 07:45_

---

_Closed by @sharkdp on 2025-05-05 11:52_

---

_Closed by @sharkdp on 2025-05-05 11:52_

---
