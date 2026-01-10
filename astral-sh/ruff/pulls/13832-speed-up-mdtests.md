```yaml
number: 13832
title: Speed up mdtests
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/speed-up-md-test
created_at: 2024-10-20T15:14:04Z
updated_at: 2024-10-21T19:13:24Z
url: https://github.com/astral-sh/ruff/pull/13832
synced_at: 2026-01-10T20:59:37Z
```

# Speed up mdtests

---

_Pull request opened by @MichaReiser on 2024-10-20 15:14_

## Summary

This PR speeds up th md tests by using a single salsa db per test suite. 
The main motivation for re-using the salsa DB is that we spend a decent amount of time re-parsing the typeshed stubs. The lower bound of any salsa test case is now around 120ms (on my machine). We should investigate if there's something we can improve to get this number down but this is beyond the scope of this PR.

The main downside of this approach is that it reduces test isolation. I do think that significant faster test execution is a fair tradeoff, especially considering that Salsa abstracts away the incremental vs "fresh" computation. 

However, it does mean that a salsa bug could lead to failing tests. In fact, I had to upgrade salsa to address a Salsa bug. 

Closes https://github.com/astral-sh/ruff/issues/13805


## Test Plan

On my machine, I get a 3x speedup.

This PR

```bash
    Starting 66 tests across 2 binaries (338 skipped)
        PASS [   0.154s] red_knot_python_semantic::mdtest mdtest::path_05_resources_mdtest_attributes_md
        PASS [   0.155s] red_knot_python_semantic::mdtest mdtest::path_02_resources_mdtest_assignment_multi_target_md
        PASS [   0.164s] red_knot_python_semantic::mdtest mdtest::path_01_resources_mdtest_assignment_annotations_md
        PASS [   0.165s] red_knot_python_semantic::mdtest mdtest::path_04_resources_mdtest_assignment_walrus_md
        PASS [   0.173s] red_knot_python_semantic::mdtest mdtest::path_06_resources_mdtest_binary_booleans_md
        PASS [   0.185s] red_knot_python_semantic::mdtest mdtest::path_08_resources_mdtest_binary_integers_md
        PASS [   0.188s] red_knot_python_semantic::mdtest mdtest::path_03_resources_mdtest_assignment_unbound_md
        PASS [   0.288s] red_knot_python_semantic::mdtest mdtest::path_07_resources_mdtest_binary_instances_md
        PASS [   0.137s] red_knot_python_semantic::mdtest mdtest::path_11_resources_mdtest_call_function_md
        PASS [   0.128s] red_knot_python_semantic::mdtest mdtest::path_13_resources_mdtest_comparison_byte_literals_md
        PASS [   0.138s] red_knot_python_semantic::mdtest mdtest::path_12_resources_mdtest_call_union_md
        PASS [   0.130s] red_knot_python_semantic::mdtest mdtest::path_15_resources_mdtest_comparison_non_boolean_returns_md
        PASS [   0.135s] red_knot_python_semantic::mdtest mdtest::path_14_resources_mdtest_comparison_integers_md
        PASS [   0.213s] red_knot_python_semantic::mdtest mdtest::path_10_resources_mdtest_call_constructor_md
        PASS [   0.216s] red_knot_python_semantic::mdtest mdtest::path_09_resources_mdtest_call_callable_instance_md
        PASS [   0.112s] red_knot_python_semantic::mdtest mdtest::path_20_resources_mdtest_conditional_if_expression_md
        PASS [   0.130s] red_knot_python_semantic::mdtest mdtest::path_18_resources_mdtest_comparison_unions_md
        PASS [   0.143s] red_knot_python_semantic::mdtest mdtest::path_19_resources_mdtest_comparison_unsupported_md
        PASS [   0.105s] red_knot_python_semantic::mdtest mdtest::path_22_resources_mdtest_conditional_match_md
        PASS [   0.178s] red_knot_python_semantic::mdtest mdtest::path_21_resources_mdtest_conditional_if_statement_md
        PASS [   0.145s] red_knot_python_semantic::mdtest mdtest::path_23_resources_mdtest_declaration_error_md
        PASS [   0.236s] red_knot_python_semantic::mdtest mdtest::path_16_resources_mdtest_comparison_strings_md
        PASS [   0.113s] red_knot_python_semantic::mdtest mdtest::path_26_resources_mdtest_exception_except_star_md
        PASS [   0.185s] red_knot_python_semantic::mdtest mdtest::path_24_resources_mdtest_exception_basic_md
        PASS [   0.100s] red_knot_python_semantic::mdtest mdtest::path_30_resources_mdtest_import_builtins_md
        PASS [   0.154s] red_knot_python_semantic::mdtest mdtest::path_27_resources_mdtest_expression_boolean_md
        PASS [   0.344s] red_knot_python_semantic::mdtest mdtest::path_17_resources_mdtest_comparison_tuples_md
        PASS [   0.134s] red_knot_python_semantic::mdtest mdtest::path_29_resources_mdtest_import_basic_md
        PASS [   0.151s] red_knot_python_semantic::mdtest mdtest::path_28_resources_mdtest_generics_md
        PASS [   0.265s] red_knot_python_semantic::mdtest mdtest::path_25_resources_mdtest_exception_control_flow_md
        PASS [   0.167s] red_knot_python_semantic::mdtest mdtest::path_31_resources_mdtest_import_conditional_md
        PASS [   0.093s] red_knot_python_semantic::mdtest mdtest::path_37_resources_mdtest_literal_collections_list_md
        PASS [   0.098s] red_knot_python_semantic::mdtest mdtest::path_36_resources_mdtest_literal_collections_dictionary_md
        PASS [   0.137s] red_knot_python_semantic::mdtest mdtest::path_32_resources_mdtest_import_errors_md
        PASS [   0.140s] red_knot_python_semantic::mdtest mdtest::path_35_resources_mdtest_literal_boolean_md
        PASS [   0.183s] red_knot_python_semantic::mdtest mdtest::path_34_resources_mdtest_import_stubs_md
        PASS [   0.126s] red_knot_python_semantic::mdtest mdtest::path_38_resources_mdtest_literal_collections_set_md
        PASS [   0.113s] red_knot_python_semantic::mdtest mdtest::path_39_resources_mdtest_literal_collections_tuple_md
        PASS [   0.231s] red_knot_python_semantic::mdtest mdtest::path_33_resources_mdtest_import_relative_md
        PASS [   0.106s] red_knot_python_semantic::mdtest mdtest::path_42_resources_mdtest_literal_float_md
        PASS [   0.116s] red_knot_python_semantic::mdtest mdtest::path_41_resources_mdtest_literal_f_string_md
        PASS [   0.210s] red_knot_python_semantic::mdtest mdtest::path_40_resources_mdtest_literal_complex_md
        PASS [   0.133s] red_knot_python_semantic::mdtest mdtest::path_45_resources_mdtest_loops_async_for_loops_md
        PASS [   0.154s] red_knot_python_semantic::mdtest mdtest::path_44_resources_mdtest_literal_string_md
        PASS [   0.140s] red_knot_python_semantic::mdtest mdtest::path_47_resources_mdtest_loops_iterators_md
        PASS [   0.139s] red_knot_python_semantic::mdtest mdtest::path_48_resources_mdtest_loops_while_loop_md
        PASS [   0.174s] red_knot_python_semantic::mdtest mdtest::path_46_resources_mdtest_loops_for_loop_md
        PASS [   0.232s] red_knot_python_semantic::mdtest mdtest::path_43_resources_mdtest_literal_integer_md
        PASS [   0.067s] red_knot_python_semantic::mdtest mdtest::path_55_resources_mdtest_shadowing_variable_declaration_md
        PASS [   0.229s] red_knot_python_semantic::mdtest mdtest::path_49_resources_mdtest_narrow_conditionals_is_md
        PASS [   0.150s] red_knot_python_semantic::mdtest mdtest::path_51_resources_mdtest_narrow_match_md
        PASS [   0.147s] red_knot_python_semantic::mdtest mdtest::path_52_resources_mdtest_scopes_builtin_md
        PASS [   0.128s] red_knot_python_semantic::mdtest mdtest::path_54_resources_mdtest_shadowing_function_md
        PASS [   0.173s] red_knot_python_semantic::mdtest mdtest::path_50_resources_mdtest_narrow_conditionals_is_not_md
        PASS [   0.163s] red_knot_python_semantic::mdtest mdtest::path_53_resources_mdtest_shadowing_class_md
        PASS [   0.156s] red_knot_python_semantic::mdtest mdtest::path_56_resources_mdtest_stubs_class_md
        PASS [   0.143s] red_knot_python_semantic::mdtest mdtest::path_57_resources_mdtest_subscript_bytes_md
        PASS [   0.138s] red_knot_python_semantic::mdtest mdtest::path_61_resources_mdtest_subscript_string_md
        PASS [   0.088s] red_knot_python_semantic::mdtest mdtest::path_64_resources_mdtest_unary_integers_md
        PASS [   0.155s] red_knot_python_semantic::mdtest mdtest::path_60_resources_mdtest_subscript_lists_md
        PASS [   0.185s] red_knot_python_semantic::mdtest mdtest::path_58_resources_mdtest_subscript_class_md
        PASS [   0.172s] red_knot_python_semantic::mdtest mdtest::path_59_resources_mdtest_subscript_instance_md
        PASS [   0.120s] red_knot_python_semantic::mdtest mdtest::path_63_resources_mdtest_unary_instance_md
        PASS [   0.153s] red_knot_python_semantic::mdtest mdtest::path_62_resources_mdtest_subscript_tuple_md
        PASS [   0.101s] red_knot_python_semantic::mdtest mdtest::path_65_resources_mdtest_unary_not_md
        PASS [   0.126s] red_knot_python_semantic::mdtest mdtest::path_66_resources_mdtest_unpacking_md
------------
     Summary [   1.389s] 66 tests run: 66 passed, 338 skipped
```

Main 

```bash
 cargo nextest run -p red_knot_python_semantic mdtest
    Finished `test` profile [unoptimized + debuginfo] target(s) in 3.91s
    Starting 66 tests across 2 binaries (338 skipped)
        PASS [   0.157s] red_knot_python_semantic::mdtest mdtest::path_06_resources_mdtest_binary_booleans_md
        PASS [   0.168s] red_knot_python_semantic::mdtest mdtest::path_02_resources_mdtest_assignment_multi_target_md
        PASS [   0.180s] red_knot_python_semantic::mdtest mdtest::path_05_resources_mdtest_attributes_md
        PASS [   0.114s] red_knot_python_semantic::mdtest mdtest::path_09_resources_mdtest_call_callable_instance_md
        PASS [   0.279s] red_knot_python_semantic::mdtest mdtest::path_03_resources_mdtest_assignment_unbound_md
        PASS [   0.120s] red_knot_python_semantic::mdtest mdtest::path_10_resources_mdtest_call_constructor_md
        PASS [   0.301s] red_knot_python_semantic::mdtest mdtest::path_04_resources_mdtest_assignment_walrus_md
        PASS [   0.360s] red_knot_python_semantic::mdtest mdtest::path_01_resources_mdtest_assignment_annotations_md
        PASS [   0.126s] red_knot_python_semantic::mdtest mdtest::path_13_resources_mdtest_comparison_byte_literals_md
        PASS [   0.139s] red_knot_python_semantic::mdtest mdtest::path_15_resources_mdtest_comparison_non_boolean_returns_md
        PASS [   0.269s] red_knot_python_semantic::mdtest mdtest::path_11_resources_mdtest_call_function_md
        PASS [   0.458s] red_knot_python_semantic::mdtest mdtest::path_08_resources_mdtest_binary_integers_md
        PASS [   0.145s] red_knot_python_semantic::mdtest mdtest::path_16_resources_mdtest_comparison_strings_md
        PASS [   0.276s] red_knot_python_semantic::mdtest mdtest::path_14_resources_mdtest_comparison_integers_md
        PASS [   0.118s] red_knot_python_semantic::mdtest mdtest::path_19_resources_mdtest_comparison_unsupported_md
        PASS [   0.340s] red_knot_python_semantic::mdtest mdtest::path_18_resources_mdtest_comparison_unions_md
        PASS [   0.556s] red_knot_python_semantic::mdtest mdtest::path_12_resources_mdtest_call_union_md
        PASS [   0.441s] red_knot_python_semantic::mdtest mdtest::path_20_resources_mdtest_conditional_if_expression_md
        PASS [   0.355s] red_knot_python_semantic::mdtest mdtest::path_22_resources_mdtest_conditional_match_md
        PASS [   0.368s] red_knot_python_semantic::mdtest mdtest::path_23_resources_mdtest_declaration_error_md
        PASS [   0.434s] red_knot_python_semantic::mdtest mdtest::path_24_resources_mdtest_exception_basic_md
        PASS [   0.348s] red_knot_python_semantic::mdtest mdtest::path_26_resources_mdtest_exception_except_star_md
        PASS [   0.876s] red_knot_python_semantic::mdtest mdtest::path_17_resources_mdtest_comparison_tuples_md
        PASS [   0.806s] red_knot_python_semantic::mdtest mdtest::path_21_resources_mdtest_conditional_if_statement_md
        PASS [   0.388s] red_knot_python_semantic::mdtest mdtest::path_28_resources_mdtest_generics_md
        PASS [   0.100s] red_knot_python_semantic::mdtest mdtest::path_30_resources_mdtest_import_builtins_md
        PASS [   0.237s] red_knot_python_semantic::mdtest mdtest::path_29_resources_mdtest_import_basic_md
        PASS [   0.230s] red_knot_python_semantic::mdtest mdtest::path_34_resources_mdtest_import_stubs_md
        PASS [   0.147s] red_knot_python_semantic::mdtest mdtest::path_35_resources_mdtest_literal_boolean_md
        PASS [   0.103s] red_knot_python_semantic::mdtest mdtest::path_36_resources_mdtest_literal_collections_dictionary_md
        PASS [   0.091s] red_knot_python_semantic::mdtest mdtest::path_37_resources_mdtest_literal_collections_list_md
        PASS [   0.436s] red_knot_python_semantic::mdtest mdtest::path_31_resources_mdtest_import_conditional_md
        PASS [   0.098s] red_knot_python_semantic::mdtest mdtest::path_38_resources_mdtest_literal_collections_set_md
        PASS [   0.085s] red_knot_python_semantic::mdtest mdtest::path_40_resources_mdtest_literal_complex_md
        PASS [   0.502s] red_knot_python_semantic::mdtest mdtest::path_32_resources_mdtest_import_errors_md
        PASS [   0.952s] red_knot_python_semantic::mdtest mdtest::path_27_resources_mdtest_expression_boolean_md
        PASS [   0.183s] red_knot_python_semantic::mdtest mdtest::path_39_resources_mdtest_literal_collections_tuple_md
        PASS [   0.104s] red_knot_python_semantic::mdtest mdtest::path_42_resources_mdtest_literal_float_md
        PASS [   1.115s] red_knot_python_semantic::mdtest mdtest::path_25_resources_mdtest_exception_control_flow_md
        PASS [   2.045s] red_knot_python_semantic::mdtest mdtest::path_07_resources_mdtest_binary_instances_md
        PASS [   0.120s] red_knot_python_semantic::mdtest mdtest::path_47_resources_mdtest_loops_iterators_md
        PASS [   0.196s] red_knot_python_semantic::mdtest mdtest::path_44_resources_mdtest_literal_string_md
        PASS [   0.209s] red_knot_python_semantic::mdtest mdtest::path_45_resources_mdtest_loops_async_for_loops_md
        PASS [   0.347s] red_knot_python_semantic::mdtest mdtest::path_41_resources_mdtest_literal_f_string_md
        PASS [   0.087s] red_knot_python_semantic::mdtest mdtest::path_51_resources_mdtest_narrow_match_md
        PASS [   0.189s] red_knot_python_semantic::mdtest mdtest::path_49_resources_mdtest_narrow_conditionals_is_md
        PASS [   0.210s] red_knot_python_semantic::mdtest mdtest::path_52_resources_mdtest_scopes_builtin_md
        PASS [   0.311s] red_knot_python_semantic::mdtest mdtest::path_48_resources_mdtest_loops_while_loop_md
        PASS [   0.305s] red_knot_python_semantic::mdtest mdtest::path_50_resources_mdtest_narrow_conditionals_is_not_md
        PASS [   0.223s] red_knot_python_semantic::mdtest mdtest::path_53_resources_mdtest_shadowing_class_md
        PASS [   0.087s] red_knot_python_semantic::mdtest mdtest::path_55_resources_mdtest_shadowing_variable_declaration_md
        PASS [   0.104s] red_knot_python_semantic::mdtest mdtest::path_56_resources_mdtest_stubs_class_md
        PASS [   0.127s] red_knot_python_semantic::mdtest mdtest::path_57_resources_mdtest_subscript_bytes_md
        PASS [   0.331s] red_knot_python_semantic::mdtest mdtest::path_54_resources_mdtest_shadowing_function_md
        PASS [   1.286s] red_knot_python_semantic::mdtest mdtest::path_33_resources_mdtest_import_relative_md
        PASS [   0.854s] red_knot_python_semantic::mdtest mdtest::path_43_resources_mdtest_literal_integer_md
        PASS [   0.126s] red_knot_python_semantic::mdtest mdtest::path_62_resources_mdtest_subscript_tuple_md
        PASS [   0.263s] red_knot_python_semantic::mdtest mdtest::path_60_resources_mdtest_subscript_lists_md
        PASS [   0.239s] red_knot_python_semantic::mdtest mdtest::path_61_resources_mdtest_subscript_string_md
        PASS [   0.155s] red_knot_python_semantic::mdtest mdtest::path_63_resources_mdtest_unary_instance_md
        PASS [   0.345s] red_knot_python_semantic::mdtest mdtest::path_59_resources_mdtest_subscript_instance_md
        PASS [   0.973s] red_knot_python_semantic::mdtest mdtest::path_46_resources_mdtest_loops_for_loop_md
        PASS [   0.248s] red_knot_python_semantic::mdtest mdtest::path_64_resources_mdtest_unary_integers_md
        PASS [   0.598s] red_knot_python_semantic::mdtest mdtest::path_58_resources_mdtest_subscript_class_md
        PASS [   0.590s] red_knot_python_semantic::mdtest mdtest::path_65_resources_mdtest_unary_not_md
        PASS [   1.626s] red_knot_python_semantic::mdtest mdtest::path_66_resources_mdtest_unpacking_md
------------
     Summary [   4.353s] 66 tests run: 66 passed, 338 skipped
```


---

_Label `red-knot` added by @MichaReiser on 2024-10-20 15:14_

---

_Review requested from @carljm by @MichaReiser on 2024-10-20 15:14_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-20 15:14_

---

_Comment by @MichaReiser on 2024-10-20 15:17_

The positive. This shows that Red Knot's incremental computation works :)

---

_Comment by @github-actions[bot] on 2024-10-20 15:39_

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

_Review comment by @AlexWaygood on `crates/red_knot_test/src/db.rs`:6 on 2024-10-20 21:08_

nit

```suggestion
use ruff_db::system::{DbWithTestSystem, System, TestSystem, SystemPath, SystemPathBuf};
```

---

_@AlexWaygood approved on 2024-10-20 21:16_

Wow. The speedup for me locally is huge as well. This looks great!

If I understand correctly, I think the tests should still be reasonably well isolated for most purposes. Just as long as we don't have some caching bug somewhere... 😆

---

_Comment by @MichaReiser on 2024-10-20 21:21_

Yes, isolation is guaranteed for as long as the test framework correctly resets the DB state after each run and salsa has no caching bugs.

---

_Comment by @MichaReiser on 2024-10-20 21:33_

The most ideal solution would be to prewarm a static db and then deep clone it for every test but salsa doesn't allow deep cloning the storage. Maybe that's something we can do once we have persistent caching 

---

_@carljm approved on 2024-10-21 17:40_

Thank you! This is a massive speedup and worth some added complexity, for sure. I have two questions I'd like to hear thoughts on:

1) We have planned features for mdtest where you can customize the workspace root and other search paths (third party paths, extra paths) for a single test or a subset of tests in a file. How do you envision supporting this? Can we make those changes safely within an existing db? If not, will we need to isolate just the tests that set these options? Or require that these things can only be customized per-test-file?

2) I think we may want an environment variable or some easy way to get back the fully isolated tests, to help us with debugging the likely future scenario where there _is_ an incremental bug (maybe in red-knot, maybe in Salsa itself) affecting the tests.

---

_Comment by @MichaReiser on 2024-10-21 19:06_

### Settings support

I don't see this as an issue. Settings will be salsa inputs and changing them invalidates the relevant queries. We already support some settings today and there are updates to update them: `Program::update_search_paths` and `Program::set_target_version`. It may require exposing some additional APIs but most should be sharable with the code used by the CLI watch mode and the LSP. 

Having said that, I'm not sure if we should allow changing all settings between tests in the same test suite. For example, we could  consider restricting the target version to apply to the entire test suite because changing the version for each test leads to large cache invalidation and slower wall time. But that also depends on how such a restriction limits the test framework ergonocmis.



---

_Merged by @MichaReiser on 2024-10-21 19:06_

---

_Closed by @MichaReiser on 2024-10-21 19:06_

---

_Branch deleted on 2024-10-21 19:06_

---

_Comment by @carljm on 2024-10-21 19:08_

> Settings will be salsa inputs and changing them invalidates the relevant queries.

That sounds great! My memory wasn't clear on how we ended up handling all the settings :)

---

_Comment by @carljm on 2024-10-21 19:09_

> I'm not sure if we should allow changing all settings between tests in the same test suite... But that also depends on how such a restriction limits the test framework ergonomics.

Yeah, I agree we'll need to see how this works out. I'm open to having some such restrictions to help us keep the test suite faster, but I think there likely may be cases where it makes the tests a lot more readable to have exactly-parallel tests for behavior in different Python versions live next to each other in the same file, rather than in separate files.

---

_Comment by @MichaReiser on 2024-10-21 19:13_

Shoot. I made a title and then forgot to cover your second question. 

My preference is to add a environment variable that allows filtering by test name. This way we get two functionalities with one env variable:

* You can set it to debug a specific test 
* You can set it see if the test passes in isolation.

We probably also want to setup tracing by calling into `ruff_db::testing::setup_logging`. I create two new issues

---
