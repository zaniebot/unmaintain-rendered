```yaml
number: 17128
title: "mdtest.py: do a full mdtest run immediately when the script is executed"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/mdtest-first-run
created_at: 2025-04-01T17:50:58Z
updated_at: 2025-04-01T18:27:57Z
url: https://github.com/astral-sh/ruff/pull/17128
synced_at: 2026-01-10T19:40:37Z
```

# mdtest.py: do a full mdtest run immediately when the script is executed

---

_Pull request opened by @AlexWaygood on 2025-04-01 17:50_

## Summary

Currently if I run `uv run crates/red_knot_python_semantic/mdtest.py` from the Ruff repo root, I get this output:

```
~/dev/ruff (main)⚡ % uv run crates/red_knot_python_semantic/mdtest.py
Ready to watch for changes...
```

...And I then have to make some spurious whitespace changes or something to a test file in order to get the script to actually run mdtest. This PR changes mdtest.py so that it does an initial run of all mdtests when you invoke the script, and _then_ starts watching for changes in test files/Rust code.

## Test Plan

Now this is the output when I run the script from a terminal:

```
~/dev/ruff (alex/mdtest-first-run)⚡ % uv run crates/red_knot_python_semantic/mdtest.py

running 195 tests
test mdtest__annotations_new_types ... ok
test mdtest__annotations_never ... ok
test mdtest__annotations_annotated ... ok
test mdtest__annotations_int_float_complex ... ok
test mdtest__annotations_starred ... ok
test mdtest__annotations_optional ... ok
test mdtest__annotations_any ... ok
test mdtest__annotations_literal ... ok
test mdtest__annotations_stdlib_typing_aliases ... ok
test mdtest__annotations_deferred ... ok
test mdtest__annotations_invalid ... ok
test mdtest__annotations_literal_string ... ok
test mdtest__annotations_callable ... ok
test mdtest__annotations_string ... ok
test mdtest__annotations_unsupported_type_qualifiers ... ok
test mdtest__annotations_union ... ok
test mdtest__annotations_unsupported_special_forms ... ok
test mdtest__assignment_unbound ... ok
test mdtest__assignment_multi_target ... ok
test mdtest__assignment_annotations ... ok
test mdtest__assignment_walrus ... ok
test mdtest__binary_classes ... ok
test mdtest__assignment_augmented ... ok
test mdtest__binary_booleans ... ok
test mdtest__binary_custom ... ok
test mdtest__binary_integers ... ok
test mdtest__binary_tuples ... ok
test mdtest__binary_unions ... ok
test mdtest__call_builtins ... ok
test mdtest__boundness_declaredness_public ... ok
test mdtest__boolean_short_circuit ... ok
test mdtest__call_annotation ... ok
test mdtest__call_constructor ... ok
test mdtest__call_callable_instance ... ok
test mdtest__binary_instances ... ok
test mdtest__call_dunder ... ok
test mdtest__call_invalid_syntax ... ok
test mdtest__call_getattr_static ... ok
test mdtest__call_function ... ok
test mdtest__call_never ... ok
test mdtest__comparison_byte_literals ... ok
test mdtest__call_subclass_of ... ok
test mdtest__attributes ... ok
test mdtest__comparison_identity ... ok
test mdtest__call_union ... ok
test mdtest__comparison_instances_membership_test ... ok
test mdtest__call_methods ... ok
test mdtest__comparison_non_bool_returns ... ok
test mdtest__comparison_integers ... ok
test mdtest__comparison_intersections ... ok
test mdtest__comparison_strings ... ok
test mdtest__comparison_instances_rich_comparison ... ok
test mdtest__comparison_unions ... ok
test mdtest__comprehensions_invalid_syntax ... ok
test mdtest__conditional_if_expression ... ok
test mdtest__comprehensions_basic ... ok
test mdtest__doc_README ... ok
test mdtest__comparison_unsupported ... ok
test mdtest__diagnostics_unpacking ... ok
test mdtest__conditional_match ... ok
test mdtest__conditional_if_statement ... ok
test mdtest__comparison_tuples ... ok
test mdtest__declaration_error ... ok
test mdtest__diagnostics_unresolved_import ... ok
test mdtest__diagnostics_no_matching_overload ... ok
test mdtest__diagnostics_attribute_assignment ... ok
test mdtest__expression_assert ... ok
test mdtest__directives_cast ... ok
test mdtest__diagnostics_invalid_argument_type ... ok
test mdtest__doc_public_type_undeclared_symbols ... ok
test mdtest__directives_assert_type ... ok
test mdtest__descriptor_protocol ... ok
test mdtest__expression_attribute ... ok
test mdtest__exception_invalid_syntax ... ok
test mdtest__exception_except_star ... ok
test mdtest__exception_control_flow ... ok
test mdtest__expression_lambda ... ok
test mdtest__expression_boolean ... ok
test mdtest__exception_basic ... ok
test mdtest__expression_if ... ok
test mdtest__final ... ok
test mdtest__function_parameters ... ok
test mdtest__generics_pep695 ... ok
test mdtest__generics_legacy ... ok
test mdtest__generics_classes ... ok
test mdtest__expression_len ... ok
test mdtest__import_builtins ... ok
test mdtest__import_conditional ... ok
test mdtest__generics_functions ... ok
test mdtest__function_return_type ... ok
test mdtest__import_case_sensitive ... ok
test mdtest__import_basic ... ok
test mdtest__generics_scoping ... ok
test mdtest__import_conflicts ... ok
test mdtest__import_conventions ... ok
test mdtest__import_errors ... ok
test mdtest__import_invalid_syntax ... ok
test mdtest__import_tracking ... ok
test mdtest__import_stubs ... ok
test mdtest__import_relative ... ok
test mdtest__known_constants ... ok
test mdtest__invalid_syntax ... ok
test mdtest__literal_boolean ... ok
test mdtest__literal_bytes ... ok
test mdtest__literal_collections_dictionary ... ok
test mdtest__literal_collections_list ... ok
test mdtest__literal_collections_tuple ... ok
test mdtest__literal_collections_set ... ok
test mdtest__mdtest_custom_typeshed ... ok
test mdtest__import_star ... ok
test mdtest__literal_complex ... ok
test mdtest__literal_f_string ... ok
test mdtest__literal_integer ... ok
test mdtest__literal_ellipsis ... ok
test mdtest__loops_async_for ... ok
test mdtest__literal_string ... ok
test mdtest__loops_iterators ... ok
test mdtest__literal_float ... ok
test mdtest__intersection_types ... ok
test mdtest__loops_while_loop ... ok
test mdtest__mdtest_config ... ok
test mdtest__narrow_conditionals_in ... ok
test mdtest__narrow_conditionals_elif_else ... ok
test mdtest__narrow_conditionals_is ... ok
test mdtest__metaclass ... ok
test mdtest__mro ... ok
test mdtest__narrow_conditionals_is_not ... ok
test mdtest__loops_for ... ok
test mdtest__narrow_boolean ... ok
test mdtest__narrow_bool_call ... ok
test mdtest__narrow_conditionals_nested ... ok
test mdtest__narrow_conditionals_boolean ... ok
test mdtest__narrow_conditionals_not ... ok
test mdtest__narrow_conditionals_not_eq ... ok
test mdtest__narrow_post_if_statement ... ok
test mdtest__narrow_while ... ok
test mdtest__regression_14334_diagnostics_in_wrong_file ... ok
test mdtest__scopes_builtin ... ok
test mdtest__narrow_isinstance ... ok
test mdtest__narrow_match ... ok
test mdtest__pep695_type_aliases ... ok
test mdtest__narrow_issubclass ... ok
test mdtest__narrow_type ... ok
test mdtest__scopes_nonlocal ... ok
test mdtest__protocols ... ok
test mdtest__scopes_moduletype_attrs ... ok
test mdtest__narrow_truthiness ... ok
test mdtest__shadowing_class ... ok
test mdtest__shadowing_variable_declaration ... ok
test mdtest__shadowing_function ... ok
test mdtest__scopes_unbound ... ok
test mdtest__scopes_eager ... ok
test mdtest__stubs_locals ... ok
test mdtest__slots ... ok
test mdtest__stubs_class ... ok
test mdtest__subscript_bytes ... ok
test mdtest__subscript_stepsize_zero ... ok
test mdtest__stubs_ellipsis ... ok
test mdtest__subscript_instance ... ok
test mdtest__subscript_lists ... ok
test mdtest__subscript_class ... ok
test mdtest__suppressions_no_type_check ... ok
test mdtest__subscript_tuple ... ok
test mdtest__subscript_string ... ok
test mdtest__suppressions_knot_ignore ... ok
test mdtest__sys_platform ... ok
test mdtest__statically_known_branches ... ok
test mdtest__suppressions_type_ignore ... ok
test mdtest__type_of_typing_dot_Type ... ok
test mdtest__type_of_dynamic ... ok
test mdtest__sys_version_info ... ok
test mdtest__terminal_statements ... ok
test mdtest__type_of_basic ... ok
test mdtest__type_api ... ok
test mdtest__type_properties_is_fully_static ... ok
test mdtest__type_properties_is_assignable_to ... ok
test mdtest__type_properties_is_disjoint_from ... ok
test mdtest__type_properties_is_single_valued ... ok
test mdtest__type_properties_is_gradual_equivalent_to ... ok
test mdtest__type_properties_is_equivalent_to ... ok
test mdtest__type_properties_str_repr ... ok
test mdtest__type_properties_is_singleton ... ok
test mdtest__type_properties_tuples_containing_never ... ok
test mdtest__type_qualifiers_classvar ... ok
test mdtest__type_qualifiers_final ... ok
test mdtest__unary_custom ... ok
test mdtest__type_properties_truthiness ... ok
test mdtest__unary_integers ... ok
test mdtest__type_properties_is_subtype_of ... ok
test mdtest__unary_invert_add_usub ... ok
test mdtest__with_async ... ok
test mdtest__unary_not ... ok
test mdtest__union_types ... ok
test mdtest__with_sync ... ok
test mdtest__unpacking ... ok

test result: ok. 195 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 1.96s

Ready to watch for changes...
```

---

_Label `testing` added by @AlexWaygood on 2025-04-01 17:50_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-01 17:50_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-01 17:50_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-01 17:50_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-01 17:50_

---

_Comment by @github-actions[bot] on 2025-04-01 17:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-04-01 17:58_

Seems fine to me, but I'm not a heavy user of `mdtest.py`, so might want to make sure @sharkdp is on board before merging.

---

_Comment by @AlexWaygood on 2025-04-01 18:02_

> Seems fine to me, but I'm not a heavy user of `mdtest.py`, so might want to make sure @sharkdp is on board before merging.

Running `mdtest.py` in an in-editor terminal (plus being able to click straight to a line that's producing an error) is great! You gotta start using it. It means you don't have to wait 10s for the whole crate to recompile just because an assertion in a MarkDown file changed.

---

_Comment by @sharkdp on 2025-04-01 18:26_

Thanks! I certainly felt the need for this as well. I think it should be the default to run the tests initially, but we could potentially add a CLI flag to skip the initial run? Sometimes I am only working on one single test suite and don't want to run any other tests. Or maybe it would be better to add some sort of filter-argument?

I suggest we just merge this and make changes if we feel it's necessary. 

---

_@sharkdp approved on 2025-04-01 18:26_

---

_Merged by @AlexWaygood on 2025-04-01 18:27_

---

_Closed by @AlexWaygood on 2025-04-01 18:27_

---

_Branch deleted on 2025-04-01 18:27_

---
