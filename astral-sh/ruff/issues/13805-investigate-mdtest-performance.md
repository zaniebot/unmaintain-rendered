```yaml
number: 13805
title: Investigate mdtest performance
type: issue
state: closed
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
created_at: 2024-10-18T07:30:47Z
updated_at: 2024-10-21T19:06:42Z
url: https://github.com/astral-sh/ruff/issues/13805
synced_at: 2026-01-10T11:09:55Z
```

# Investigate mdtest performance

---

_Issue opened by @MichaReiser on 2024-10-18 07:30_

I haven't done any profiling but I'm surprised that the fastest tests take 0.15s to run and the slowest is close to 2s! 

```
❯ cargo nextest run -p red_knot_python_semantic mdtest
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.08s
    Starting 59 tests across 2 binaries (279 skipped; run ID: 90252fab-1bb0-4ac8-b6a7-c103d28f5f9e, nextest profile: default)
        PASS [   0.142s] red_knot_python_semantic::mdtest mdtest::path_07_resources_mdtest_call_callable_instance_md
        PASS [   0.142s] red_knot_python_semantic::mdtest mdtest::path_14_resources_mdtest_comparison_strings_md
        PASS [   0.145s] red_knot_python_semantic::mdtest mdtest::path_13_resources_mdtest_comparison_non_boolean_returns_md
        PASS [   0.157s] red_knot_python_semantic::mdtest mdtest::path_05_resources_mdtest_attributes_md
        PASS [   0.158s] red_knot_python_semantic::mdtest mdtest::path_16_resources_mdtest_comparison_unsupported_md
        PASS [   0.171s] red_knot_python_semantic::mdtest mdtest::path_11_resources_mdtest_comparison_byte_literals_md
        PASS [   0.175s] red_knot_python_semantic::mdtest mdtest::path_08_resources_mdtest_call_constructor_md
        PASS [   0.194s] red_knot_python_semantic::mdtest mdtest::path_02_resources_mdtest_assignment_multi_target_md
        PASS [   0.237s] red_knot_python_semantic::mdtest mdtest::path_04_resources_mdtest_assignment_walrus_md
        PASS [   0.259s] red_knot_python_semantic::mdtest mdtest::path_06_resources_mdtest_binary_integers_md
        PASS [   0.132s] red_knot_python_semantic::mdtest mdtest::path_27_resources_mdtest_import_errors_md
        PASS [   0.286s] red_knot_python_semantic::mdtest mdtest::path_01_resources_mdtest_assignment_annotations_md
        PASS [   0.174s] red_knot_python_semantic::mdtest mdtest::path_25_resources_mdtest_import_builtins_md
        PASS [   0.316s] red_knot_python_semantic::mdtest mdtest::path_12_resources_mdtest_comparison_integers_md
        PASS [   0.170s] red_knot_python_semantic::mdtest mdtest::path_30_resources_mdtest_literal_boolean_md
        PASS [   0.184s] red_knot_python_semantic::mdtest mdtest::path_31_resources_mdtest_literal_collections_dictionary_md
        PASS [   0.187s] red_knot_python_semantic::mdtest mdtest::path_32_resources_mdtest_literal_collections_list_md
        PASS [   0.335s] red_knot_python_semantic::mdtest mdtest::path_24_resources_mdtest_import_basic_md
        PASS [   0.172s] red_knot_python_semantic::mdtest mdtest::path_33_resources_mdtest_literal_collections_set_md
        PASS [   0.417s] red_knot_python_semantic::mdtest mdtest::path_03_resources_mdtest_assignment_unbound_md
        PASS [   0.365s] red_knot_python_semantic::mdtest mdtest::path_20_resources_mdtest_declaration_error_md
        PASS [   0.429s] red_knot_python_semantic::mdtest mdtest::path_09_resources_mdtest_call_function_md
        PASS [   0.162s] red_knot_python_semantic::mdtest mdtest::path_35_resources_mdtest_literal_complex_md
        PASS [   0.289s] red_knot_python_semantic::mdtest mdtest::path_29_resources_mdtest_import_stubs_md
        PASS [   0.425s] red_knot_python_semantic::mdtest mdtest::path_19_resources_mdtest_conditional_match_md
        PASS [   0.430s] red_knot_python_semantic::mdtest mdtest::path_22_resources_mdtest_exception_except_star_md
        PASS [   0.182s] red_knot_python_semantic::mdtest mdtest::path_37_resources_mdtest_literal_float_md
        PASS [   0.360s] red_knot_python_semantic::mdtest mdtest::path_26_resources_mdtest_import_conditional_md
        PASS [   0.545s] red_knot_python_semantic::mdtest mdtest::path_17_resources_mdtest_conditional_if_expression_md
        PASS [   0.122s] red_knot_python_semantic::mdtest mdtest::path_48_resources_mdtest_shadowing_class_md
        PASS [   0.185s] red_knot_python_semantic::mdtest mdtest::path_42_resources_mdtest_loops_iterators_md
        PASS [   0.337s] red_knot_python_semantic::mdtest mdtest::path_34_resources_mdtest_literal_collections_tuple_md
        PASS [   0.108s] red_knot_python_semantic::mdtest mdtest::path_50_resources_mdtest_shadowing_variable_declaration_md
        PASS [   0.173s] red_knot_python_semantic::mdtest mdtest::path_46_resources_mdtest_narrow_match_md
        PASS [   0.162s] red_knot_python_semantic::mdtest mdtest::path_47_resources_mdtest_narrow_not_none_md
        PASS [   0.310s] red_knot_python_semantic::mdtest mdtest::path_39_resources_mdtest_literal_string_md
        PASS [   0.601s] red_knot_python_semantic::mdtest mdtest::path_21_resources_mdtest_exception_basic_md
        PASS [   0.184s] red_knot_python_semantic::mdtest mdtest::path_51_resources_mdtest_stubs_class_md
        PASS [   0.328s] red_knot_python_semantic::mdtest mdtest::path_40_resources_mdtest_loops_async_for_loops_md
        PASS [   0.702s] red_knot_python_semantic::mdtest mdtest::path_10_resources_mdtest_call_union_md
        PASS [   0.208s] red_knot_python_semantic::mdtest mdtest::path_52_resources_mdtest_subscript_bytes_md
        PASS [   0.437s] red_knot_python_semantic::mdtest mdtest::path_36_resources_mdtest_literal_f_string_md
        PASS [   0.140s] red_knot_python_semantic::mdtest mdtest::path_56_resources_mdtest_subscript_tuple_md
        PASS [   0.251s] red_knot_python_semantic::mdtest mdtest::path_49_resources_mdtest_shadowing_function_md
        PASS [   0.326s] red_knot_python_semantic::mdtest mdtest::path_44_resources_mdtest_narrow_conditionals_is_md
        PASS [   0.367s] red_knot_python_semantic::mdtest mdtest::path_45_resources_mdtest_narrow_conditionals_is_not_md
        PASS [   0.238s] red_knot_python_semantic::mdtest mdtest::path_54_resources_mdtest_subscript_instance_md
        PASS [   0.421s] red_knot_python_semantic::mdtest mdtest::path_43_resources_mdtest_loops_while_loop_md
        PASS [   0.257s] red_knot_python_semantic::mdtest mdtest::path_55_resources_mdtest_subscript_string_md
        PASS [   0.293s] red_knot_python_semantic::mdtest mdtest::path_57_resources_mdtest_unary_integers_md
        PASS [   0.928s] red_knot_python_semantic::mdtest mdtest::path_18_resources_mdtest_conditional_if_statement_md
        PASS [   0.910s] red_knot_python_semantic::mdtest mdtest::path_23_resources_mdtest_expression_boolean_md
        PASS [   0.978s] red_knot_python_semantic::mdtest mdtest::path_15_resources_mdtest_comparison_tuples_md
        PASS [   0.501s] red_knot_python_semantic::mdtest mdtest::path_53_resources_mdtest_subscript_class_md
        PASS [   0.787s] red_knot_python_semantic::mdtest mdtest::path_38_resources_mdtest_literal_integer_md
        PASS [   0.787s] red_knot_python_semantic::mdtest mdtest::path_41_resources_mdtest_loops_for_loop_md
        PASS [   1.069s] red_knot_python_semantic::mdtest mdtest::path_28_resources_mdtest_import_relative_md
        PASS [   0.719s] red_knot_python_semantic::mdtest mdtest::path_58_resources_mdtest_unary_not_md
        PASS [   1.861s] red_knot_python_semantic::mdtest mdtest::path_59_resources_mdtest_unpacking_md
------------
     Summary [   2.465s] 59 tests run: 59 passed, 279 skipped

```

For comparison, running all ruff linter tests:

```
Summary [   4.408s] 2110 tests run: 2110 passed, 4 skipped
```

---

_Label `red-knot` added by @MichaReiser on 2024-10-18 07:30_

---

_Label `testing` added by @AlexWaygood on 2024-10-18 09:32_

---

_Comment by @carljm on 2024-10-21 17:56_

Also see #13835 

---

_Closed by @MichaReiser on 2024-10-21 19:06_

---
