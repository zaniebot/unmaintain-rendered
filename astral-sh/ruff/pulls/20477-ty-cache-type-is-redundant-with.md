```yaml
number: 20477
title: "[ty] cache Type::is_redundant_with"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/recinv
created_at: 2025-09-18T22:24:42Z
updated_at: 2025-10-16T11:46:57Z
url: https://github.com/astral-sh/ruff/pull/20477
synced_at: 2026-01-10T17:34:34Z
```

# [ty] cache Type::is_redundant_with

---

_Pull request opened by @carljm on 2025-09-18 22:24_

## Summary

TODO

## Test Plan

Added test that stack overflowed before this PR.


---

_Label `ty` added by @carljm on 2025-09-18 22:25_

---

_Comment by @github-actions[bot] on 2025-09-18 22:26_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-16 11:31:13.724737373 +0000
+++ new-output.txt	2025-10-16 11:31:17.072755647 +0000
@@ -1,5 +1,5 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/execute.rs:402:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(d017)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/execute.rs:402:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(16443)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/execute.rs:402:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(d417)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/execute.rs:402:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(16c43)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-09-18 22:35_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Frecinv)

### Merging #20477 will **improve performances by 12.52%**

<sub>Comparing <code>cjm/recinv</code> (081f95a) with <code>main</code> (5fb1423)</sub>



### Summary

`⚡ 4` improvements  
`✅ 17` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | Instrumentation | [`` attrs ``](https://codspeed.io/astral-sh/ruff/branches/cjm%2Frecinv?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Aattrs%3A%3Aproject%3A%3Aattrs&runnerMode=Instrumentation) | 382.1 ms | 362.7 ms | +5.36% |
| ⚡ | Instrumentation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/cjm%2Frecinv?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation) | 852 ms | 757.2 ms | +12.52% |
| ⚡ | WallTime | [`` small[altair] ``](https://codspeed.io/astral-sh/ruff/branches/cjm%2Frecinv?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Baltair%5D&runnerMode=WallTime) | 2.4 s | 2.3 s | +5.25% |
| ⚡ | WallTime | [`` small[freqtrade] ``](https://codspeed.io/astral-sh/ruff/branches/cjm%2Frecinv?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Bfreqtrade%5D&runnerMode=WallTime) | 4.5 s | 4.2 s | +7.32% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/cjm%2Frecinv?sectionId=benchmark-comparison-section-baseline-result-skipped).


---

_Comment by @codspeed-hq[bot] on 2025-09-18 22:35_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Frecinv?runnerMode=WallTime)

### Merging #20477 will **improve performances by 80.76%**

<sub>Comparing <code>cjm/recinv</code> (5ee2420) with <code>main</code> (130a794)</sub>



### Summary

`⚡ 1` improvement  
`✅ 7` untouched  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` small[freqtrade] `` | 9.3 s | 5.2 s | +80.76% |


---

_Comment by @ibraheemdev on 2025-09-23 16:24_

(Rebasing this on main to see the benchmark results)

---

_Comment by @MichaReiser on 2025-09-24 09:26_

I took a look at the mypy primer hang for pandas-stubs and I'm starting to believe that this is the same as astral-sh/ty#1111. 

```
  2025-09-24T09:03:00.068040Z DEBUG salsa::function::execute: infer_deferred_types(Id(164be)): execute: result.revisions = QueryRevisions {
    changed_at: R2,
    durability: Durability::LOW,
    origin: Derived(
        [
            Input(
                parsed_module(Id(c5a)),
            ),
            Input(
                semantic_index(Id(c5a)),
            ),
            Input(
                Definition.kind(Id(164be)),
            ),
            Input(
                File.path(Id(c5a)),
            ),
            Input(
                infer_definition_types(Id(16402)),
            ),
            Input(
                infer_definition_types(Id(16490)),
            ),
            Input(
                ClassLiteral < 'db >::pep695_generic_context_(Id(8c3d)),
            ),
            Input(
                ClassLiteral < 'db >::explicit_bases_(Id(8c3d)),
            ),
            Input(
                infer_definition_types(Id(e3ea)),
            ),
            Input(
                ClassLiteral < 'db >::is_typed_dict_(Id(8c05)),
            ),
            Input(
                ClassLiteral < 'db >::pep695_generic_context_(Id(8c05)),
            ),
            Input(
                ClassLiteral < 'db >::explicit_bases_(Id(8c05)),
            ),
            Input(
                GenericContext(Id(14c04)),
            ),
            Input(
                Specialization(Id(11423)),
            ),
            Input(
                GenericAlias(Id(11823)),
            ),
            Input(
                TupleType(Id(d81b)),
            ),
            Input(
                ClassLiteral < 'db >::try_mro_::interned_arguments(Id(12c30)),
            ),
            Input(
                ClassLiteral < 'db >::try_mro_(Id(12c30)),
            ),
            Input(
                Specialization(Id(10c3d)),
            ),
            Input(
                GenericAlias(Id(1103d)),
            ),
            Input(
                infer_definition_types(Id(1645a)),
            ),
            Input(
                ClassLiteral < 'db >::pep695_generic_context_(Id(8c3a)),
            ),
            Input(
                ClassLiteral < 'db >::explicit_bases_(Id(8c3a)),
            ),
            Input(
                infer_definition_types(Id(e3ed)),
            ),
            Input(
                Type < 'db >::member_lookup_with_policy_::interned_arguments(Id(5058)),
            ),
            Input(
                Type < 'db >::member_lookup_with_policy_(Id(5058)),
            ),
            Input(
                ModuleNameIngredient(Id(3002)),
            ),
            Input(
                resolve_module_query(Id(3002)),
            ),
            Input(
                File.path(Id(c90)),
            ),
            Input(
                global_scope(Id(c90)),
            ),
            Input(
                place_table(Id(6e2c)),
            ),
            Input(
                place_by_id::interned_arguments(Id(880b)),
            ),
            Input(
                place_by_id(Id(880b)),
            ),
            Input(
                ClassLiteral < 'db >::pep695_generic_context_(Id(a804)),
            ),
            Input(
                ClassLiteral < 'db >::explicit_bases_(Id(a804)),
            ),
            Input(
                ClassLiteral < 'db >::is_typed_dict_(Id(a804)),
            ),
            Input(
                Type < 'db >::is_subtype_of_::interned_arguments(Id(ec6e)),
            ),
            Input(
                Type < 'db >::is_subtype_of_(Id(ec6e)),
            ),
            Input(
                Type < 'db >::member_lookup_with_policy_::interned_arguments(Id(5059)),
            ),
            Input(
                Type < 'db >::member_lookup_with_policy_(Id(5059)),
            ),
            Input(
                ClassLiteral < 'db >::try_mro_::interned_arguments(Id(12c32)),
            ),
            Input(
                ClassLiteral < 'db >::try_mro_(Id(12c32)),
            ),
            Input(
                Project.check_mode(Id(800)),
            ),
            Input(
                Project.file_set(Id(800)),
            ),
            Input(
                file_settings(Id(c5a)),
            ),
            Input(
                Project.settings(Id(800)),
            ),
            Input(
                suppressions(Id(c5a)),
            ),
            Input(
                ClassLiteral < 'db >::is_typed_dict_(Id(8c08)),
            ),
            Input(
                ClassLiteral < 'db >::pep695_generic_context_(Id(8c08)),
            ),
            Input(
                ClassLiteral < 'db >::explicit_bases_(Id(8c08)),
            ),
            Input(
                ClassLiteral < 'db >::try_mro_::interned_arguments(Id(12c2f)),
            ),
            Input(
                place_by_id::interned_arguments(Id(880d)),
            ),
            Input(
                place_by_id(Id(880d)),
            ),
            Input(
                ClassLiteral < 'db >::pep695_generic_context_(Id(a808)),
            ),
            Input(
                ClassLiteral < 'db >::explicit_bases_(Id(a808)),
            ),
            Input(
                ClassLiteral < 'db >::try_mro_(Id(12c2f)),
            ),
            Input(
                ClassLiteral < 'db >::decorators_(Id(8c07)),
            ),
            Input(
                enum_metadata(Id(8c07)),
            ),
            Input(
                Specialization(Id(10c4c)),
            ),
            Input(
                GenericAlias(Id(1104c)),
            ),
            Input(
                Specialization(Id(10c71)),
            ),
            Input(
                GenericAlias(Id(11071)),
            ),
        ],
    ),
    verified_final: false,
    extra: QueryRevisionsExtra(
        Some(
            QueryRevisionsExtraInner {
                tracked_struct_ids: [],
                cycle_heads: CycleHeads(
                    [
                        CycleHead {
                            database_key_index: ClassLiteral < 'db >::is_typed_dict_(Id(8c06)),
                            iteration_count: IterationCount(
                                0,
                            ),
                        },
                        CycleHead {
                            database_key_index: ClassLiteral < 'db >::try_mro_(Id(12c21)),
                            iteration_count: IterationCount(
                                0,
                            ),
                        },
                        CycleHead {
                            database_key_index: ClassLiteral < 'db >::try_mro_(Id(12c24)),
                            iteration_count: IterationCount(
                                21,
                            ),
                        },
                        CycleHead {
                            database_key_index: Type < 'db >::is_subtype_of_(Id(ec62)),
                            iteration_count: IterationCount(
                                0,
                            ),
                        },
                        CycleHead {
                            database_key_index: ClassLiteral < 'db >::try_mro_(Id(12c2f)),
                            iteration_count: IterationCount(
                                1,
                            ),
                        },
                    ],
                ),
                iteration: IterationCount(
                    0,
                ),
            },
        ),
    ),
}
    at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/3713cd7/src/function/execute.rs:259 on ThreadId(3)
    in salsa::function::fetch::fetch with query=infer_deferred_types(Id(164be))
    in salsa::function::fetch::fetch with query=ClassLiteral < 'db >::explicit_bases_(Id(8c3c))
    in salsa::function::fetch::fetch with query=ClassLiteral < 'db >::try_mro_(Id(12c2f))
    in salsa::function::fetch::fetch with query=Type < 'db >::is_subtype_of_(Id(ec70))
    in salsa::function::fetch::fetch with query=infer_expression_types_impl(Id(2e96))
    in salsa::function::fetch::fetch with query=infer_definition_types(Id(1644a))
    in salsa::function::fetch::fetch with query=infer_deferred_types(Id(16474))
    in salsa::function::fetch::fetch with query=ClassLiteral < 'db >::explicit_bases_(Id(8c39))
    in salsa::function::fetch::fetch with query=infer_deferred_types(Id(16475))
    in salsa::function::fetch::fetch with query=ClassLiteral < 'db >::explicit_bases_(Id(8c2f))
    in salsa::function::fetch::fetch with query=infer_deferred_types(Id(164bd))
    in salsa::function::fetch::fetch with query=ClassLiteral < 'db >::explicit_bases_(Id(8c2d))
    in salsa::function::fetch::fetch with query=ClassLiteral < 'db >::try_mro_(Id(12c24))
    in salsa::function::fetch::fetch with query=Type < 'db >::is_subtype_of_(Id(ec62))
    in salsa::function::fetch::fetch with query=infer_expression_types_impl(Id(2e9c))
    in salsa::function::fetch::fetch with query=infer_definition_types(Id(16497))
    in salsa::function::fetch::fetch with query=infer_deferred_types(Id(164ad))
    in salsa::function::fetch::fetch with query=ClassLiteral < 'db >::explicit_bases_(Id(8c2b))
    in salsa::function::fetch::fetch with query=infer_deferred_types(Id(164ae))
    in salsa::function::fetch::fetch with query=ClassLiteral < 'db >::explicit_bases_(Id(8c29))
    in salsa::function::fetch::fetch with query=ClassLiteral < 'db >::try_mro_(Id(12c21))
    in salsa::function::fetch::fetch with query=ClassLiteral < 'db >::is_typed_dict_(Id(8c06))
    in salsa::function::fetch::fetch with query=infer_deferred_types(Id(285e))
    in salsa::function::fetch::fetch with query=infer_scope_types(Id(2402))
    in salsa::function::fetch::fetch with query=check_file_impl(Id(c17))
```

The cycle head is `ClassLiteral < 'db >::is_typed_dict_(Id(8c06))` is the 4th outer most query. We not only have nested cycles (of which some run 21 times!), but the cycle itself is also just massive. 


I've been able to reproduce the hang on almost every run using

```
TY_MAX_PARALLELISM=2 /Users/micha/astral/ruff/target/debug/ty check pandas-stubs tests --python /tmp/mypy_primer/projects/_pandas-stubs_venv --output-format concise
```

But I haven't yet been able to reproduce it after pulling in https://github.com/astral-sh/ruff/pull/20515 

What I don't understand is why this is only happening when ty runs multi-threaded. Is it just because the cycle nesting is different and, depending on the nesting, the cycle converges faster? Or is there a multithreaded bug my hacky "re-use nested fixpoint values" implementation only happens to bypass the problematic code?


---

_Comment by @MichaReiser on 2025-09-24 10:37_

I added some logging to the `try_mro` recovery function. 

The `DatetimeIndex` is the outer `try_mro` call and what we can see is that the MRO never converges. It keeps flipping between two states (I haven't verified that it's just two but we reach the same state after iteration 122 and 124).

MRO for Name("DatetimeIndex"): Iteration 122 vs 123 https://www.diffchecker.com/EJa9XVe0/
MRO 123 vs 124: https://www.diffchecker.com/SAEY5RqG/
MRO 122 vs 124 https://www.diffchecker.com/IeUISYud/

I suspect what's happening is that, depending on the entry query, the `try_mro` cycle handling converges or it keeps flipping, giving the impression that ty hangs.

In fact, the query does panic fairly quickly. I added a few logging statements to Salsa and the output for a run where ty hangs is:

```
✦3 ❯ TY_LOG=salsa=warn TY_MAX_PARALLELISM=2 /Users/micha/astral/ruff/target/release/ty check pandas-stubs  --python /tmp/mypy_primer/projects/_pandas-stubs_venv --output-format concise
Checking ------------------------------------------------------------ 6/179 files                                                                                                                                                                WARN ClassLiteral < 'db >::try_mro_(Id(1342c)): execute: too many cycle iterations
WARN Query panicked
WARN Query panicked
WARN Query panicked
WARN Query panicked
Checking ------------------------------------------------------------ 18/179 files
```

which confirms that there's still a multi threaded bug in Salsa when a query participating in a cycle panics (as discussed here https://github.com/awslabs/shuttle/issues/193#issuecomment-3180469470). 

For now, I suggest that we add some tracing logs to Salsa similar to what I did so that this becomes easier to discover (and doesn't require a 3h debugging session). I'll spend some time today to see if I can figure out why Salsa ends up hanging if there's a panic but I don't plan on spending too much time on it because astral-sh/ty#1111 seems more important than fixing something that improves our debugging experience.

---

_Comment by @MichaReiser on 2025-09-24 11:09_

The fix that I've in mind for astral-sh/ty#1111 might help here because we would iterate all cycles at once, rather than having many nested cycles. I think that could help because I suspect that whether we run into this issue depends on how the `try_mro` calls are nested.

---

_Comment by @MichaReiser on 2025-09-24 16:12_

I bumped the Salsa version to a temporary commit that contains the upstream fix and mypy primer is now failing with:

```
fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4c9d669/src/function/execute.rs:217:25 when checking `/tmp/mypy_primer/projects/pandas-stubs/pandas-stubs/core/indexes/period.pyi`: `ClassLiteral < 'db >::try_mro_(Id(1fc13)): execute: too many cycle iterations`
```

I'll consider this "solved" from the salsa side (except for the optimisation discussed in astral-sh/ty#1111 which I expect to help here). @carljm ping me if you want me to look into anything else.

---

_Comment by @github-actions[bot] on 2025-09-24 16:20_

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

_Comment by @github-actions[bot] on 2025-09-30 10:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 52 diagnostics
+ Found 51 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct metadata = ~8MB
+     struct metadata = ~9MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~236MB
+ TOTAL MEMORY USAGE: ~247MB
-     struct fields = ~13MB
+     struct fields = ~14MB

prefect (https://github.com/PrefectHQ/prefect)
-     struct metadata = ~28MB
+     struct metadata = ~31MB
-     memo metadata = ~93MB
+     memo metadata = ~98MB

```
</details>


---

_Comment by @MichaReiser on 2025-09-30 10:21_

Nice, mypy primer now completes with the new salsa version... The test failure is somewhat concerning. Is this new due to the salsa update?

---

_Comment by @MichaReiser on 2025-09-30 10:22_

TODO: Address `+ WARN expected `heap_size` to be provided by Salsa query `Type < 'db >::is_subtype_of_``

---

_Comment by @MichaReiser on 2025-09-30 10:25_

> Nice, mypy primer now completes with the new salsa version... The test failure is somewhat concerning. Is this new due to the salsa update?

Nope, the test was failing before my update commit. @carljm maybe something you could take a look at (I've no idea what's happening there 😆 )

---

_Comment by @sharkdp on 2025-09-30 10:51_

> Nope, the test was failing before my update commit. @carljm maybe something you could take a look at (I've no idea what's happening there 😆 )

The `ConstraintSet` failures look similar to https://github.com/astral-sh/ruff/pull/20528#issuecomment-3348203667. Maybe also something for @dcreager.

There's also a panic in recursive protocol tests, though :-|

---

_Comment by @MichaReiser on 2025-09-30 10:51_

My biggest concern with this PR is the memory increase

```diff
-     memo metadata = ~89MB
+     memo metadata = ~103MB
```

That's pretty substantial and can easily be a few GB for large projects (I suspect it uses around as much memory as `infer_expression_types` simply because there are so many cached results. 

Could we limit the caching scope? Would it even make sense to cache `when_subtype_of`?

---

_Comment by @carljm on 2025-10-01 16:08_

> Would it even make sense to cache `when_subtype_of`?

I expect this would be worse for memory usage, as the return type of `when_subtype_of` is significantly larger than the return type of `is_subtype_of`.

It may be interesting to look at this again after https://github.com/astral-sh/ruff/pull/20602 and only cache `is_redundant_in_union_with`.

---

_Comment by @dcreager on 2025-10-01 21:13_

> The `ConstraintSet` failures look similar to [#20528 (comment)](https://github.com/astral-sh/ruff/pull/20528#issuecomment-3348203667). Maybe also something for @dcreager.

These test failures should be fixed by https://github.com/astral-sh/ruff/pull/20653. We had some test output that depended on the ordering of Salsa IDs

---

_Comment by @dcreager on 2025-10-01 21:15_

> It may be interesting to look at this again after #20602 and only cache `is_redundant_in_union_with`.

Caching `is_subtype_of` will be important (or at least presumably helpful) for https://github.com/astral-sh/ruff/pull/20093, since to build up constraint sets involving typevars, we'll have to do a lot of subtype checks on the lower/upper bounds.

---

_Comment by @MichaReiser on 2025-10-10 11:21_

Hmm, I did a rebase and one test is now stack overflowing. @carljm do you remember if this already happened before? I don't think it's something I should be able to mess up with salsa. Or did I mess up the rebase?

<details>

```
hashbrown::raw::RawTable<T,A>::reserve_rehash mod.rs:977
hashbrown::raw::RawTable<T,A>::reserve mod.rs:940
hashbrown::raw::RawTable<T,A>::find_or_find_insert_slot mod.rs:1146
hashbrown::table::HashTable<T,A>::entry table.rs:365
indexmap::map::core::IndexMapCore<K,V>::insert_full core.rs:341
indexmap::map::IndexMap<K,V,S>::insert_full map.rs:464
indexmap::map::IndexMap<K,V,S>::insert map.rs:447
indexmap::set::IndexSet<T,S>::insert set.rs:382
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:82
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
core::ops::function::FnMut::call_mut function.rs:168
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::normalized_impl types.rs:10895
ty_python_semantic::types::Type::normalized_impl::{{closure}} types.rs:1243
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::normalized_impl types.rs:1243
ty_python_semantic::types::Type::normalized types.rs:1237
ty_python_semantic::types::Type::is_equivalent_to_impl types.rs:2225
ty_python_semantic::types::Type::when_equivalent_to types.rs:2192
ty_python_semantic::types::Type::is_equivalent_to types.rs:2188
ty_python_semantic::types::generics::Specialization::materialize_impl::{{closure}} generics.rs:925
[Inlined] core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &mut F>::call_once function.rs:313
[Inlined] core::option::Option<T>::map option.rs:1158
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::next map.rs:107
alloc::vec::Vec<T,A>::extend_desugared mod.rs:3741
<alloc::vec::Vec<T,A> as alloc::vec::spec_extend::SpecExtend<T,I>>::spec_extend spec_extend.rs:19
<alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter spec_from_iter_nested.rs:42
<alloc::vec::Vec<T> as alloc::vec::spec_from_iter::SpecFromIter<T,I>>::from_iter spec_from_iter.rs:34
<alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter mod.rs:3633
core::iter::traits::iterator::Iterator::collect iterator.rs:2027
alloc::boxed::iter::<impl core::iter::traits::collect::FromIterator<I> for alloc::boxed::Box<[I]>>::from_iter iter.rs:144
core::iter::traits::iterator::Iterator::collect iterator.rs:2027
ty_python_semantic::types::generics::Specialization::materialize_impl generics.rs:932
ty_python_semantic::types::generics::Specialization::apply_type_mapping_impl generics.rs:809
ty_python_semantic::types::class::GenericAlias::apply_type_mapping_impl class.rs:295
ty_python_semantic::types::class::ClassType::apply_type_mapping_impl class.rs:484
ty_python_semantic::types::instance::NominalInstanceType::apply_type_mapping_impl instance.rs:487
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6421
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6478
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:640
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::UnionBuilder::push_type builder.rs:505
ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl builder.rs:453
ty_python_semantic::types::builder::UnionBuilder::add_in_place builder.rs:256
ty_python_semantic::types::builder::UnionBuilder::add builder.rs:250
ty_python_semantic::types::UnionType::from_elements::{{closure}} types.rs:10718
core::iter::adapters::map::map_fold::{{closure}} map.rs:88
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold macros.rs:255
<core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold map.rs:128
ty_python_semantic::types::UnionType::from_elements types.rs:10717
ty_python_semantic::types::UnionType::map types.rs:10762
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6477
ty_python_semantic::types::Type::apply_type_mapping_impl::{{closure}} types.rs:6501
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::apply_type_mapping_impl types.rs:6501
ty_python_semantic::types::Type::materialize types.rs:961
ty_python_semantic::types::Type::top_materialization types.rs:896
ty_python_semantic::types::generics::is_subtype_in_invariant_position generics.rs:560
ty_python_semantic::types::generics::has_relation_in_invariant_position generics.rs:680
ty_python_semantic::types::generics::Specialization::has_relation_to_impl generics.rs:995
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}}::{{closure}} class.rs:580
ty_python_semantic::types::constraints::ConstraintSet::and constraints.rs:208
ty_python_semantic::types::class::ClassType::has_relation_to_impl::{{closure}} class.rs:579
<I as ty_python_semantic::types::constraints::IteratorConstraintsExtension<T>>::when_any constraints.rs:128
ty_python_semantic::types::class::ClassType::has_relation_to_impl class.rs:562
ty_python_semantic::types::instance::NominalInstanceType::has_relation_to_impl instance.rs:383
ty_python_semantic::types::Type::has_relation_to_impl::{{closure}} types.rs:2146
ty_python_semantic::types::cyclic::CycleDetector<Tag,T,R>::visit cyclic.rs:86
ty_python_semantic::types::Type::has_relation_to_impl types.rs:2145
ty_python_semantic::types::Type::has_relation_to types.rs:1493
ty_python_semantic::types::Type::is_redundant_with types.rs:1483
ty_python_semantic::types::builder::InnerIntersectionBuilder::add_positive builder.rs:924
ty_python_semantic::types::builder::IntersectionBuilder::add_positive_impl builder.rs:689
ty_python_semantic::types::builder::IntersectionBuilder::add_positive builder.rs:596
core::ops::function::FnMut::call_mut function.rs:168
core::iter::traits::double_ended::DoubleEndedIterator::rfold double_ended.rs:308
<core::iter::adapters::rev::Rev<I> as core::iter::traits::iterator::Iterator>::fold rev.rs:83
ty_python_semantic::semantic_index::use_def::ConstraintsIterator::narrow use_def.rs:746
ty_python_semantic::place::place_from_bindings_impl::{{closure}} place.rs:1057
core::ops::function::impls::<impl core::ops::function::FnMut<A> for &mut F>::call_mut function.rs:301
core::iter::traits::iterator::Iterator::find_map::check::{{closure}} iterator.rs:2914
<core::iter::adapters::peekable::Peekable<I> as core::iter::traits::iterator::Iterator>::try_fold peekable.rs:100
core::iter::traits::iterator::Iterator::find_map iterator.rs:2920
<core::iter::adapters::filter_map::FilterMap<I,F> as core::iter::traits::iterator::Iterator>::next filter_map.rs:64
ty_python_semantic::place::place_from_bindings_impl place.rs:1061
ty_python_semantic::place::place_from_bindings place.rs:462
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_local_place_load builder.rs:6936
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_place_load builder.rs:6956
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_name_load builder.rs:6843
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_name_expression builder.rs:7305
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_expression_impl builder.rs:5543
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_all_argument_types builder.rs:5379
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_call_expression_impl builder.rs:6538
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_call_expression builder.rs:6355
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_expression_impl builder.rs:5553
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_region_expression builder.rs:1261
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_region builder.rs:462
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::finish_expression builder.rs:9650
<ty_python_semantic::types::infer::infer_expression_types_impl::infer_expression_types_impl_Configuration_ as salsa::function::Configuration>::execute::inner_ infer.rs:218
<ty_python_semantic::types::infer::infer_expression_types_impl::infer_expression_types_impl_Configuration_ as salsa::function::Configuration>::execute setup_tracked_fn.rs:302
salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_query execute.rs:473
salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_maybe_iterate execute.rs:186
salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute execute.rs:101
salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold fetch.rs:206
salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold_with_retry fetch.rs:107
salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}} fetch.rs:64
core::option::Option<T>::or_else option.rs:1647
[Inlined] salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo fetch.rs:63
salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch fetch.rs:30
ty_python_semantic::types::infer::infer_expression_types_impl::{{closure}} setup_tracked_fn.rs:478
salsa::attach::Attached::attach attach.rs:79
salsa::attach::attach::{{closure}} attach.rs:135
std::thread::local::LocalKey<T>::try_with local.rs:315
std::thread::local::LocalKey<T>::with local.rs:279
salsa::attach::attach attach.rs:133
ty_python_semantic::types::infer::infer_expression_types_impl setup_tracked_fn.rs:468
ty_python_semantic::types::infer::infer_expression_types infer.rs:190
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_standalone_expression_impl builder.rs:5505
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_maybe_standalone_expression builder.rs:5483
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_statement builder.rs:2032
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_body builder.rs:2017
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_if_statement builder.rs:2675
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_statement builder.rs:2034
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_body builder.rs:2017
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_function_body builder.rs:1890
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_region_scope builder.rs:474
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_region builder.rs:458
ty_python_semantic::types::infer::builder::TypeInferenceBuilder::finish_scope builder.rs:9783
<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_ as salsa::function::Configuration>::execute::inner_ infer.rs:80
<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_ as salsa::function::Configuration>::execute setup_tracked_fn.rs:302
salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_query execute.rs:473
salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_maybe_iterate execute.rs:186
salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute execute.rs:101
salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold fetch.rs:206
salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold_with_retry fetch.rs:107
salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}} fetch.rs:64
core::option::Option<T>::or_else option.rs:1647
[Inlined] salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo fetch.rs:63
salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch fetch.rs:30
ty_python_semantic::types::infer::infer_scope_types::{{closure}} setup_tracked_fn.rs:478
salsa::attach::Attached::attach attach.rs:79
salsa::attach::attach::{{closure}} attach.rs:135
std::thread::local::LocalKey<T>::try_with local.rs:315
std::thread::local::LocalKey<T>::with local.rs:279
salsa::attach::attach attach.rs:133
ty_python_semantic::types::infer::infer_scope_types setup_tracked_fn.rs:468
<ruff_python_ast::generated::ExprRef as ty_python_semantic::semantic_model::HasType>::inferred_type semantic_model.rs:367
<ruff_python_ast::generated::ExprCall as ty_python_semantic::semantic_model::HasType>::inferred_type semantic_model.rs:377
<ruff_python_ast::generated::Expr as ty_python_semantic::semantic_model::HasType>::inferred_type semantic_model.rs:436
<ty_python_semantic::pull_types::PullTypesVisitor as ruff_python_ast::visitor::source_order::SourceOrderVisitor>::visit_expr pull_types.rs:104
ruff_python_ast::generated::StmtIf::visit_source_order generated.rs:9902
ruff_python_ast::generated::Stmt::visit_source_order generated.rs:4725
ruff_python_ast::visitor::source_order::walk_stmt source_order.rs:219
<ty_python_semantic::pull_types::PullTypesVisitor as ruff_python_ast::visitor::source_order::SourceOrderVisitor>::visit_stmt pull_types.rs:100
ruff_python_ast::visitor::source_order::walk_body source_order.rs:208
ruff_python_ast::visitor::source_order::SourceOrderVisitor::visit_body source_order.rs:146
ruff_python_ast::generated::StmtFunctionDef::visit_source_order generated.rs:9700
ruff_python_ast::generated::Stmt::visit_source_order generated.rs:4715
ruff_python_ast::visitor::source_order::walk_stmt source_order.rs:219
<ty_python_semantic::pull_types::PullTypesVisitor as ruff_python_ast::visitor::source_order::SourceOrderVisitor>::visit_stmt pull_types.rs:100
ruff_python_ast::visitor::source_order::walk_body source_order.rs:208
ruff_python_ast::visitor::source_order::SourceOrderVisitor::visit_body source_order.rs:146
ty_python_semantic::pull_types::pull_types pull_types.rs:17
core::ops::function::FnOnce::call_once function.rs:253
ty_test::attempt_test::{{closure}} lib.rs:603
std::panicking::catch_unwind::do_call panicking.rs:589
__rust_try 0x00000001028f4d7c
[Inlined] std::panicking::catch_unwind panicking.rs:552
std::panic::catch_unwind panic.rs:359
ruff_db::panic::catch_unwind panic.rs:145
ty_test::attempt_test lib.rs:603
ty_test::run_test::{{closure}} lib.rs:326
core::ops::function::impls::<impl core::ops::function::FnMut<A> for &mut F>::call_mut function.rs:301
<core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::find_map macros.rs:342
<core::iter::adapters::filter_map::FilterMap<I,F> as core::iter::traits::iterator::Iterator>::next filter_map.rs:64
<alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter spec_from_iter_nested.rs:25
<alloc::vec::Vec<T> as alloc::vec::spec_from_iter::SpecFromIter<T,I>>::from_iter spec_from_iter.rs:34
<alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter mod.rs:3633
core::iter::traits::iterator::Iterator::collect iterator.rs:2027
ty_test::run_test lib.rs:386
ty_test::run lib.rs:72
mdtest::mdtest mdtest.rs:29
mdtest::mdtest__pep695_type_aliases mdtest.rs:7
mdtest::mdtest__pep695_type_aliases::{{closure}} mdtest.rs:10
core::ops::function::FnOnce::call_once function.rs:253
[Inlined] core::ops::function::FnOnce::call_once function.rs:253
test::__rust_begin_short_backtrace lib.rs:648
[Inlined] test::run_test_in_process::{{closure}} lib.rs:671
[Inlined] <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once unwind_safe.rs:272
[Inlined] std::panicking::catch_unwind::do_call panicking.rs:589
[Inlined] std::panicking::catch_unwind panicking.rs:552
[Inlined] std::panic::catch_unwind panic.rs:359
[Inlined] test::run_test_in_process lib.rs:671
test::run_test::{{closure}} lib.rs:592
[Inlined] test::run_test::{{closure}} lib.rs:622
std::sys::backtrace::__rust_begin_short_backtrace backtrace.rs:158
[Inlined] std::thread::Builder::spawn_unchecked_::{{closure}}::{{closure}} mod.rs:559
[Inlined] <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once unwind_safe.rs:272
[Inlined] std::panicking::catch_unwind::do_call panicking.rs:589
[Inlined] std::panicking::catch_unwind panicking.rs:552
[Inlined] std::panic::catch_unwind panic.rs:359
[Inlined] std::thread::Builder::spawn_unchecked_::{{closure}} mod.rs:557
core::ops::function::FnOnce::call_once{{vtable.shim}} function.rs:253
[Inlined] <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once boxed.rs:1971
std::sys::pal::unix::thread::Thread::new::thread_start thread.rs:107
_pthread_start 0x000000019c2bbc0c
```

</details>

---

_Comment by @MichaReiser on 2025-10-10 11:48_

The memory usage now looks better but it also isn't a perf improvement anymore

---

_Comment by @AlexWaygood on 2025-10-10 11:49_

> Hmm, I did a rebase and one test is now stack overflowing. @carljm do you remember if this already happened before? I don't think it's something I should be able to mess up with salsa. Or did I mess up the rebase?

it might be because Carl now needs to cache `Type::is_redundant_with` rather than (or as well as?) `Type::is_subtype_of`, following the recent changes to how union simplification is done

---

_Comment by @carljm on 2025-10-10 16:07_

> The memory usage now looks better but it also isn't a perf improvement anymore

Pretty sure that both of these changes are because union simplification now uses `is_redundant_with`, so `is_subtype_of` is not that much used anymore. If we change this PR to cache `is_redundant_with` (which will be necessary for it to serve the original cycle-catching purpose) then I expect the memory usage increase and perf improvement to both return.

Not sure about the stack overflow. I wouldn't expect this PR in and of itself to cause a stack overflow.

---

_Comment by @AlexWaygood on 2025-10-10 16:09_

> Not sure about the stack overflow. I wouldn't expect this PR in and of itself to cause a stack overflow.

but the mdtest you've added as part of this PR causes us to overflow on `main`, and I expect that because you now need to cache `is_redundant_with` rather than `is_subtype_of`, this PR no longer fixes that overflow

---

_Comment by @carljm on 2025-10-10 16:22_

> but the mdtest you've added as part of this PR causes us to overflow on `main`, and I expect that because you now need to cache `is_redundant_with` rather than `is_subtype_of`, this PR no longer fixes that overflow

Oh yes, of course! Totally forgot I'd added that test in this PR.

---

_Renamed from "[ty] cache Type::is_subtype_of" to "[ty] cache Type::is_redundant_with" by @AlexWaygood on 2025-10-14 15:19_

---

_Comment by @AlexWaygood on 2025-10-14 15:35_

I switched the PR to cache `is_redundant_with` rather than `is_subtype_of`. As expected, the performance improvement and memory-usage regression have both returned (though the improvement on freqtrade isn't quite as... dramatic... as it used to be, now that https://github.com/astral-sh/ruff/pull/20650 and https://github.com/astral-sh/ruff/pull/20602 have both landed 😆.

There does also seem to be a new fuzzer failure on seed 102, though.

---

_Comment by @MichaReiser on 2025-10-14 16:29_

This PR resolves some hangs. So I'm fine accepting the memory regression, and we can look into adding support for within a revision LRU to salsa (for queries that return a cloneable value).



---

_Comment by @MichaReiser on 2025-10-14 16:44_

The fuzzer failure should be fixed with the latest salsa version (of my fixpoint rewrite PR)

---

_Marked ready for review by @ibraheemdev on 2025-10-14 17:11_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-10-14 17:11_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-10-14 17:11_

---

_Review requested from @dcreager by @ibraheemdev on 2025-10-14 17:11_

---

_Comment by @MichaReiser on 2025-10-16 11:33_

Added a tracking issue https://github.com/astral-sh/ty/issues/1367

---

_@AlexWaygood approved on 2025-10-16 11:35_

let's go

---

_Merged by @MichaReiser on 2025-10-16 11:46_

---

_Closed by @MichaReiser on 2025-10-16 11:46_

---

_Branch deleted on 2025-10-16 11:46_

---
