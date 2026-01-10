```yaml
number: 2199
title: "Speed-up `ty_python_semantic` compile time"
type: issue
state: open
author: MichaReiser
labels:
  - internal
assignees: []
created_at: 2025-12-24T07:55:54Z
updated_at: 2025-12-24T08:01:23Z
url: https://github.com/astral-sh/ty/issues/2199
synced_at: 2026-01-10T01:56:41Z
```

# Speed-up `ty_python_semantic` compile time

---

_Issue opened by @MichaReiser on 2025-12-24 07:55_

`ty_python_semantic` is by far the slowest to-compile crate in ty (and it's in the critical path too). 

What stands out to me from looking at the timing report (`RUSTC_BOOTSTRAP=1 RUSTFLAGS="-Ztime-passes" cargo build -p ty_python_semantic`)

```
   Compiling ty_python_semantic v0.0.0 (/Users/micha/astral/ruff/crates/ty_python_semantic)
time:   0.000; rss:  313MB ->  313MB (   +0MB)	find_cgu_reuse
time:   0.000; rss:   73MB ->   74MB (   +1MB)	parse_crate
time:   0.000; rss:   75MB ->   75MB (   +0MB)	incr_comp_prepare_session_directory
time:   0.000; rss:   75MB ->   75MB (   +0MB)	incr_comp_garbage_collect_session_directories
time:   0.000; rss:   80MB ->   80MB (   +0MB)	crate_injection
time:   0.534; rss:  313MB ->  397MB (  +84MB)	codegen_to_LLVM_IR
time:   0.543; rss:  308MB ->  397MB (  +89MB)	codegen_crate
time:   0.000; rss:  398MB ->  398MB (   +0MB)	assert_dep_graph
time:   0.000; rss:  398MB ->  398MB (   +1MB)	incr_comp_persist_dep_graph
time:   0.474; rss:  335MB ->  397MB (  +62MB)	LLVM_passes
time:   0.009; rss:  399MB ->  395MB (   -3MB)	encode_query_results
time:   0.010; rss:  399MB ->  395MB (   -3MB)	incr_comp_serialize_result_cache
time:   0.010; rss:  398MB ->  395MB (   -3MB)	incr_comp_persist_result_cache
time:   0.010; rss:  398MB ->  395MB (   -2MB)	serialize_dep_graph
time:   0.000; rss:  312MB ->  312MB (   +0MB)	join_worker_thread
time:   0.035; rss:  312MB ->  312MB (   +0MB)	copy_all_cgu_workproducts_to_incr_comp_cache_dir
time:   0.035; rss:  312MB ->  312MB (   +0MB)	finish_ongoing_codegen
time:   0.000; rss:  312MB ->  312MB (   +0MB)	serialize_work_products
time:   0.012; rss:  312MB ->  314MB (   +2MB)	link_rlib
time:   0.013; rss:  312MB ->  314MB (   +2MB)	link_binary
time:   0.013; rss:  312MB ->  313MB (   +1MB)	link_crate
time:   0.014; rss:  312MB ->  313MB (   +1MB)	link
time:   1.572; rss:   21MB ->  117MB (  +97MB)	total
time:   0.520; rss:   80MB ->  297MB ( +217MB)	expand_crate
time:   0.520; rss:   80MB ->  297MB ( +217MB)	macro_expand_crate
time:   0.006; rss:  297MB ->  297MB (   +0MB)	AST_validation
time:   0.002; rss:  297MB ->  298MB (   +1MB)	finalize_imports
time:   0.001; rss:  298MB ->  298MB (   +0MB)	compute_effective_visibilities
time:   0.000; rss:  298MB ->  298MB (   +0MB)	lint_reexports
time:   0.004; rss:  298MB ->  299MB (   +1MB)	finalize_macro_resolutions
time:   0.079; rss:  299MB ->  343MB (  +44MB)	late_resolve_crate
time:   0.005; rss:  343MB ->  343MB (   +0MB)	resolve_check_unused
time:   0.006; rss:  343MB ->  343MB (   +0MB)	resolve_postprocess
time:   0.098; rss:  297MB ->  343MB (  +46MB)	resolve_crate
time:   0.004; rss:  343MB ->  344MB (   +0MB)	write_dep_info
time:   0.003; rss:  344MB ->  344MB (   +0MB)	complete_gated_feature_checking
time:   0.024; rss:  424MB ->  424MB (   +0MB)	drop_ast
time:   0.000; rss:  406MB ->  406MB (   +0MB)	looking_for_entry_point
time:   0.001; rss:  406MB ->  406MB (   +0MB)	looking_for_derive_registrar
time:   0.036; rss:  406MB ->  402MB (   -5MB)	misc_checking_1
time:   0.464; rss:  402MB ->  517MB ( +115MB)	coherence_checking
time:   1.725; rss:  402MB ->  678MB ( +276MB)	type_check_crate
time:   1.829; rss:  678MB ->  893MB ( +215MB)	MIR_borrow_checking
time:   0.054; rss:  898MB ->  901MB (   +2MB)	module_lints
time:   0.054; rss:  898MB ->  901MB (   +2MB)	lint_checking
time:   0.053; rss:  901MB ->  901MB (   +1MB)	privacy_checking_modules
time:   0.000; rss:  901MB ->  901MB (   +0MB)	check_lint_expectations
time:   0.141; rss:  893MB ->  901MB (   +8MB)	misc_checking_3
time:   0.002; rss:  956MB ->  956MB (   +0MB)	monomorphization_collector_root_collections
time:   1.801; rss:  956MB -> 1250MB ( +295MB)	monomorphization_collector_graph_walk
time:   0.248; rss: 1253MB -> 1290MB (  +37MB)	partition_and_assert_distinct_symbols
time:   2.255; rss:  901MB -> 1259MB ( +358MB)	generate_crate_metadata
time:   0.000; rss: 1264MB -> 1265MB (   +0MB)	find_cgu_reuse
time:   5.518; rss: 1265MB -> 1806MB ( +541MB)	codegen_to_LLVM_IR
time:   5.532; rss: 1259MB -> 1806MB ( +547MB)	codegen_crate
time:   0.000; rss: 1806MB -> 1806MB (   +0MB)	assert_dep_graph
time:   0.000; rss: 1806MB -> 1807MB (   +0MB)	incr_comp_persist_dep_graph
time:   4.966; rss: 1437MB -> 1803MB ( +366MB)	LLVM_passes
time:   0.110; rss: 1807MB -> 1810MB (   +3MB)	encode_query_results
time:   0.124; rss: 1807MB -> 1797MB (  -10MB)	incr_comp_serialize_result_cache
time:   0.124; rss: 1807MB -> 1797MB (  -10MB)	incr_comp_persist_result_cache
time:   0.124; rss: 1806MB -> 1797MB (   -9MB)	serialize_dep_graph
time:   0.000; rss: 1246MB -> 1246MB (   +0MB)	join_worker_thread
time:   0.070; rss: 1246MB -> 1246MB (   +0MB)	copy_all_cgu_workproducts_to_incr_comp_cache_dir
time:   0.070; rss: 1246MB -> 1246MB (   +0MB)	finish_ongoing_codegen
time:   0.000; rss: 1246MB -> 1246MB (   +0MB)	serialize_work_products
time:   0.117; rss: 1237MB -> 1244MB (   +6MB)	link_rlib
time:   0.119; rss: 1237MB -> 1244MB (   +6MB)	link_binary
time:   0.119; rss: 1237MB -> 1228MB (   -9MB)	link_crate
time:   0.120; rss: 1246MB -> 1228MB (  -18MB)	link
time:  12.826; rss:   21MB ->  127MB ( +107MB)	total
```

is that rustcs spends about 2s doing monomorphization:

```
time:   1.801; rss:  956MB -> 1250MB ( +295MB)	monomorphization_collector_graph_walk
```

which I suspect also leads to a lot of extra work in `codegen` and `LLVM_passes`. 

The output of `cargo llvm-lines` stronlgy suggest that a lot of monomorphization happens in Salsa tracked functions (not very surprisingly)

```
  Lines                  Copies               Function name
  -----                  ------               -------------
  3501590                88125                (TOTAL)
   128787 (3.7%,  3.7%)   2004 (2.3%,  2.3%)  std::thread::local::LocalKey<T>::try_with
   125930 (3.6%,  7.3%)   3598 (4.1%,  6.4%)  tracing_core::dispatcher::get_default::{{closure}}
   119935 (3.4%, 10.7%)     71 (0.1%,  6.4%)  salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_maybe_iterate
    97824 (2.8%, 13.5%)   1278 (1.5%,  7.9%)  salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_maybe_iterate::{{closure}}
    67890 (1.9%, 15.4%)     73 (0.1%,  8.0%)  salsa::interned::IngredientImpl<C>::intern_id
    59367 (1.7%, 17.1%)   1799 (2.0%, 10.0%)  tracing_core::dispatcher::get_default
    44009 (1.3%, 18.4%)   1842 (2.1%, 12.1%)  core::result::Result<T,E>::unwrap_or_else
    43455 (1.2%, 19.6%)     87 (0.1%, 12.2%)  <core::iter::adapters::flatten::FlattenCompat<I,U> as core::iter::traits::iterator::Iterator>::size_hint
    42811 (1.2%, 20.8%)    528 (0.6%, 12.8%)  core::iter::traits::iterator::Iterator::try_fold
    41180 (1.2%, 22.0%)    568 (0.6%, 13.4%)  salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_maybe_iterate::{{closure}}::{{closure}}
    39400 (1.1%, 23.1%)    560 (0.6%, 14.1%)  salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold_cycle::{{closure}}
    39129 (1.1%, 24.3%)   2035 (2.3%, 16.4%)  core::ops::function::FnOnce::call_once
    34399 (1.0%, 25.2%)    352 (0.4%, 16.8%)  <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
    33331 (1.0%, 26.2%)     71 (0.1%, 16.9%)  salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
    29296 (0.8%, 27.0%)     70 (0.1%, 16.9%)  salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold_cycle
    28480 (0.8%, 27.9%)     71 (0.1%, 17.0%)  salsa::function::maybe_changed_after::<impl salsa::function::IngredientImpl<C>>::deep_verify_memo
    28158 (0.8%, 28.7%)     71 (0.1%, 17.1%)  salsa::function::maybe_changed_after::<impl salsa::function::IngredientImpl<C>>::maybe_changed_after_cold
    27173 (0.8%, 29.4%)    257 (0.3%, 17.4%)  <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold
    26128 (0.7%, 30.2%)    284 (0.3%, 17.7%)  salsa::function::maybe_changed_after::<impl salsa::function::IngredientImpl<C>>::shallow_verify_memo::{{closure}}
```

Most notably `execute_maybe_iterate`, which accounts for 3.4% of all the code. 

We should look into how we can reduce the amount of monomorphized code in Salsa to improve compile times. 

Upstream issue https://github.com/salsa-rs/salsa/issues/1042


---

_Label `internal` added by @MichaReiser on 2025-12-24 07:56_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-24 07:56_

---
