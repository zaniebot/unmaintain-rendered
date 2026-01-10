```yaml
number: 15584
title: Document fix safety
type: issue
state: open
author: dylwil3
labels:
  - documentation
assignees: []
created_at: 2025-01-19T17:38:21Z
updated_at: 2025-09-11T13:53:23Z
url: https://github.com/astral-sh/ruff/issues/15584
synced_at: 2026-01-10T11:09:57Z
```

# Document fix safety

---

_Issue opened by @dylwil3 on 2025-01-19 17:38_

The rules found in the locations listed below need to have a `## Fix safety` section added to their doc comment to explain whether and why their fix is sometimes or always unsafe.

If you're interested in picking any of these up then first of all - thank you, it's a big help! To help make the review process quick and get your PR merged, please include the following information in your PR:

1. (In your PR summary) A link to the PR where the fix was introduced and/or made unsafe
2. (In your PR summary) If present, a link to a comment in the original PR or in the codebase where some reasoning is given around the safety
3. In your PR summary) If possible, a code sample demonstrating the unsafety
4. (In the section on fix safety) Please distinguish between whether the fix is _always_ unsafe or _sometimes_ unsafe.
5. (In your section on fix safety) Be sure to double check grammar and spelling!

Thank you again!

<details><summary>Checklist of rules</summary>

- [x] crates/ruff_linter/src/rules/ruff/rules/quadratic_list_summation.rs
- [x] crates/ruff_linter/src/rules/ruff/rules/invalid_formatter_suppression_comment.rs (https://github.com/astral-sh/ruff/pull/17722)
- [x] crates/ruff_linter/src/rules/ruff/rules/collection_literal_concatenation.rs
- [ ] crates/ruff_linter/src/rules/ruff/rules/implicit_optional.rs
- [x] crates/ruff_linter/src/rules/ruff/rules/unnecessary_round.rs
- [x] crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs
- [x] crates/ruff_linter/src/rules/ruff/rules/zip_instead_of_pairwise.rs (https://github.com/astral-sh/ruff/pull/17755)
- [ ] crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs
- [x] crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs
- [ ] crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs
- [x] crates/ruff_linter/src/rules/pylint/rules/modified_iterating_set.rs (https://github.com/astral-sh/ruff/pull/17824)
- [x] crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs (https://github.com/astral-sh/ruff/pull/17825)
- [ ] crates/ruff_linter/src/rules/pylint/rules/sys_exit_alias.rs
- [x] crates/ruff_linter/src/rules/pylint/rules/nested_min_max.rs (https://github.com/astral-sh/ruff/pull/17878)
- [ ] crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs
- [ ] crates/ruff_linter/src/rules/pylint/rules/repeated_equality_comparison.rs
- [ ] crates/ruff_linter/src/rules/pycodestyle/rules/lambda_assignment.rs
- [ ] crates/ruff_linter/src/rules/pycodestyle/rules/trailing_whitespace.rs
- [ ] crates/ruff_linter/src/rules/flake8_pytest_style/rules/assertion.rs
- [ ] crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs
- [ ] crates/ruff_linter/src/rules/flake8_pytest_style/rules/marks.rs
- [ ] crates/ruff_linter/src/rules/flake8_pie/rules/duplicate_class_field_definition.rs
- [ ] crates/ruff_linter/src/rules/flake8_return/rules/function.rs
- [x] crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs (https://github.com/astral-sh/ruff/pull/17652)
- [ ] crates/ruff_linter/src/rules/flake8_bugbear/rules/unused_loop_control_variable.rs
- [ ] crates/ruff_linter/src/rules/flake8_logging/rules/invalid_get_logger_argument.rs
- [ ] crates/ruff_linter/src/rules/flake8_logging/rules/direct_logger_instantiation.rs
- [ ] crates/ruff_linter/src/rules/flake8_pyi/rules/docstring_in_stubs.rs
- [ ] crates/ruff_linter/src/rules/pydocstyle/rules/one_liner.rs
- [ ] crates/ruff_linter/src/rules/pydocstyle/rules/ends_with_period.rs
- [ ] crates/ruff_linter/src/rules/pydocstyle/rules/ends_with_punctuation.rs
- [ ] crates/ruff_linter/src/rules/pydocstyle/rules/backslashes.rs
- [ ] crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs
- [ ] crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs
- [ ] crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs
- [x] crates/ruff_linter/src/rules/flake8_simplify/rules/needless_bool.rs (https://github.com/astral-sh/ruff/pull/18086)
- [ ] crates/ruff_linter/src/rules/flake8_simplify/rules/ast_with.rs
- [ ] crates/ruff_linter/src/rules/flake8_simplify/rules/collapsible_if.rs
- [ ] crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_dict_get.rs
- [ ] crates/ruff_linter/src/rules/flake8_simplify/rules/suppressible_exception.rs
- [ ] crates/ruff_linter/src/rules/flake8_simplify/rules/ast_bool_op.rs
- [ ] crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs
- [ ] crates/ruff_linter/src/rules/flake8_simplify/rules/reimplemented_builtin.rs
- [ ] crates/ruff_linter/src/rules/flake8_simplify/rules/ast_ifexp.rs
- [ ] crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_if_exp.rs
- [ ] crates/ruff_linter/src/rules/flake8_import_conventions/rules/unconventional_import_alias.rs
- [ ] crates/ruff_linter/src/rules/flake8_annotations/rules/definition.rs
- [ ] crates/ruff_linter/src/rules/flake8_raise/rules/unnecessary_paren_on_raise_exception.rs
- [ ] crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs
- [ ] crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs
- [ ] crates/ruff_linter/src/rules/fastapi/rules/fastapi_redundant_response_model.rs
- [ ] crates/ruff_linter/src/rules/pandas_vet/rules/inplace_argument.rs
- [ ] crates/ruff_linter/src/rules/refurb/rules/unnecessary_enumerate.rs
- [ ] crates/ruff_linter/src/rules/refurb/rules/check_and_remove_from_set.rs
- [ ] crates/ruff_linter/src/rules/refurb/rules/repeated_append.rs
- [ ] crates/ruff_linter/src/rules/refurb/rules/print_empty_string.rs
- [ ] crates/ruff_linter/src/rules/refurb/rules/delete_full_slice.rs
- [ ] crates/ruff_linter/src/rules/refurb/rules/implicit_cwd.rs
- [x] crates/ruff_linter/src/rules/refurb/rules/readlines_in_for.rs
- [x] crates/ruff_linter/src/rules/flake8_async/rules/long_sleep_not_forever.rs
- [ ] crates/ruff_linter/src/rules/flake8_errmsg/rules/string_in_exception.rs
- [ ] crates/ruff_linter/src/rules/flake8_tidy_imports/rules/relative_imports.rs
- [ ] crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_builtin_import.rs
- [ ] crates/ruff_linter/src/rules/pyupgrade/rules/use_pep695_type_alias.rs
- [ ] crates/ruff_linter/src/rules/pyupgrade/rules/use_pep604_isinstance.rs
- [x] crates/ruff_linter/src/rules/pyupgrade/rules/format_literals.rs (https://github.com/astral-sh/ruff/pull/17443)
- [x] crates/ruff_linter/src/rules/pyupgrade/rules/outdated_version_block.rs (https://github.com/astral-sh/ruff/pull/17444)
- [x] crates/ruff_linter/src/rules/pyupgrade/rules/replace_stdout_stderr.rs (https://github.com/astral-sh/ruff/pull/17441)
- [x] crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs (https://github.com/astral-sh/ruff/pull/17441)
- [x] crates/ruff_linter/src/rules/pyflakes/rules/repeated_keys.rs (https://github.com/astral-sh/ruff/pull/17440)
- [x] crates/ruff_linter/src/rules/pyflakes/rules/unused_variable.rs (https://github.com/astral-sh/ruff/pull/17410)

</details>




---

_Label `documentation` added by @dylwil3 on 2025-01-19 17:38_

---

_Label `good first issue` added by @dylwil3 on 2025-01-19 17:38_

---

_Label `help wanted` added by @dylwil3 on 2025-01-19 17:38_

---

_Comment by @InSyncWithFoo on 2025-01-19 19:46_

A linked version of the list:

<details>
<ul>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/quadratic_list_summation.rs"><code>quadratic_list_summation.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/invalid_formatter_suppression_comment.rs"><code>invalid_formatter_suppression_comment.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/collection_literal_concatenation.rs"><code>collection_literal_concatenation.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/implicit_optional.rs"><code>implicit_optional.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/unnecessary_round.rs"><code>unnecessary_round.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs"><code>missing_fstring_syntax.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/zip_instead_of_pairwise.rs"><code>zip_instead_of_pairwise.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs"><code>post_init_default.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs"><code>static_join_to_fstring.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs"><code>useless_import_alias.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pylint/rules/modified_iterating_set.rs"><code>modified_iterating_set.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs"><code>unnecessary_dunder_call.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pylint/rules/sys_exit_alias.rs"><code>sys_exit_alias.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pylint/rules/nested_min_max.rs"><code>nested_min_max.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs"><code>unspecified_encoding.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pylint/rules/repeated_equality_comparison.rs"><code>repeated_equality_comparison.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pycodestyle/rules/lambda_assignment.rs"><code>lambda_assignment.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pycodestyle/rules/trailing_whitespace.rs"><code>trailing_whitespace.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_pytest_style/rules/assertion.rs"><code>assertion.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs"><code>fixture.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_pytest_style/rules/marks.rs"><code>marks.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_pie/rules/duplicate_class_field_definition.rs"><code>duplicate_class_field_definition.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_return/rules/function.rs"><code>function.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs"><code>mutable_argument_default.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_bugbear/rules/unused_loop_control_variable.rs"><code>unused_loop_control_variable.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_logging/rules/invalid_get_logger_argument.rs"><code>invalid_get_logger_argument.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_logging/rules/direct_logger_instantiation.rs"><code>direct_logger_instantiation.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_pyi/rules/docstring_in_stubs.rs"><code>docstring_in_stubs.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pydocstyle/rules/one_liner.rs"><code>one_liner.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pydocstyle/rules/ends_with_period.rs"><code>ends_with_period.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pydocstyle/rules/ends_with_punctuation.rs"><code>ends_with_punctuation.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pydocstyle/rules/backslashes.rs"><code>backslashes.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs"><code>typing_only_runtime_import.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs"><code>runtime_import_in_type_checking_block.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs"><code>numpy_2_0_deprecation.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/needless_bool.rs"><code>needless_bool.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/ast_with.rs"><code>ast_with.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/collapsible_if.rs"><code>collapsible_if.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_dict_get.rs"><code>if_else_block_instead_of_dict_get.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/suppressible_exception.rs"><code>suppressible_exception.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/ast_bool_op.rs"><code>ast_bool_op.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs"><code>ast_expr.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/reimplemented_builtin.rs"><code>reimplemented_builtin.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/ast_ifexp.rs"><code>ast_ifexp.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_if_exp.rs"><code>if_else_block_instead_of_if_exp.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_import_conventions/rules/unconventional_import_alias.rs"><code>unconventional_import_alias.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_annotations/rules/definition.rs"><code>definition.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_raise/rules/unnecessary_paren_on_raise_exception.rs"><code>unnecessary_paren_on_raise_exception.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs"><code>manual_list_comprehension.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs"><code>fastapi_non_annotated_dependency.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/fastapi/rules/fastapi_redundant_response_model.rs"><code>fastapi_redundant_response_model.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pandas_vet/rules/inplace_argument.rs"><code>inplace_argument.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/refurb/rules/unnecessary_enumerate.rs"><code>unnecessary_enumerate.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/refurb/rules/check_and_remove_from_set.rs"><code>check_and_remove_from_set.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/refurb/rules/repeated_append.rs"><code>repeated_append.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/refurb/rules/print_empty_string.rs"><code>print_empty_string.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/refurb/rules/delete_full_slice.rs"><code>delete_full_slice.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/refurb/rules/implicit_cwd.rs"><code>implicit_cwd.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/refurb/rules/readlines_in_for.rs"><code>readlines_in_for.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_async/rules/long_sleep_not_forever.rs"><code>long_sleep_not_forever.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_errmsg/rules/string_in_exception.rs"><code>string_in_exception.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_tidy_imports/rules/relative_imports.rs"><code>relative_imports.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_builtin_import.rs"><code>unnecessary_builtin_import.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyupgrade/rules/use_pep695_type_alias.rs"><code>use_pep695_type_alias.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyupgrade/rules/use_pep604_isinstance.rs"><code>use_pep604_isinstance.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyupgrade/rules/format_literals.rs"><code>format_literals.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyupgrade/rules/outdated_version_block.rs"><code>outdated_version_block.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyupgrade/rules/replace_stdout_stderr.rs"><code>replace_stdout_stderr.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs"><code>super_call_with_parameters.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyflakes/rules/repeated_keys.rs"><code>repeated_keys.rs</code></a></li>
    <li><a href="https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyflakes/rules/unused_variable.rs"><code>unused_variable.rs</code></a></li>
</ul>
</details>

---

_Comment by @sanwalsulehri on 2025-01-21 16:45_

can i do this

---

_Comment by @MichaReiser on 2025-01-21 17:06_

@sanwalsulehri sure, feel free to tackle them one by one

---

_Comment by @sanwalsulehri on 2025-01-21 17:52_

yeah sure

---

_Comment by @dhruvmanila on 2025-01-22 05:06_

Please comment here on which rule you're going to tackle to avoid duplicate work :)

---

_Comment by @Kalmaegi on 2025-04-15 15:56_

I'd like to give it a try, starting from the `unused_variable.rs`.

---

_Comment by @VascoSch92 on 2025-04-19 16:15_

Hey, 

I have a small question. I was looking into `invalid_formatter_suppression_comments.rs`, and I saw that the fix is always unsafe. I tried to understand why and I came to this [comment](https://github.com/astral-sh/ruff/pull/9899#issue-2125918197) in the PR #9899, which say that the fix is always marked unsafe. But I don't get why, and I cannot find an example where this is unsafe :-( 

Perhaps, because it can be that the comment has nothing to do with ruff and it had another purpose, or?

---

_Comment by @MichaReiser on 2025-04-22 07:37_

Thanks for working on all the fix safety documentation and going through old PRs to unravel the reason! 


> Perhaps, because it can be that the comment has nothing to do with ruff and it had another purpose, or?

Looking at the rule, I think the reason for making the fix unsafe is that there are two possible fixes:

a) Remove the suppression because it isn't suppressing anything
b) Move the suppression to a valid position



---

_Comment by @VascoSch92 on 2025-04-29 19:59_

I will take care of the `ruff` rules ;-)

@dylwil3 could you update the list once you have time? :-) Just to take track what there is still to do.

For instance, these are done:

- [x] crates/ruff_linter/src/rules/ruff/rules/quadratic_list_summation.rs #17480 

- [x] crates/ruff_linter/src/rules/ruff/rules/collection_literal_concatenation.rs #17484 
- [x] crates/ruff_linter/src/rules/ruff/rules/unnecessary_round.rs #17483 
- [x] crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs #17485 
- [x] crates/ruff_linter/src/rules/ruff/rules/invalid_formatter_suppression_comment.rs #17722 
- [x] crates/ruff_linter/src/rules/ruff/rules/implicit_optional.rs #17759 
- [x] crates/ruff_linter/src/rules/ruff/rules/zip_instead_of_pairwise.rs #17755 
- [x] crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs #17760

---

_Comment by @dylwil3 on 2025-05-02 10:35_

@VascoSch92 / @Kalmaegi thank you both so much for the PRs! I've just updated the top post here to give a few tips for making these PRs quicker to review. For your current open PRs, would it be possible for you to follow that guide for each of your PR summaries and ping me from each PR once it's ready? Sorry for the extra homework, but I think it'll help move these along!

---

_Comment by @yunchipang on 2025-05-02 23:06_

I'll work on pylint rules 🙋🏻‍♀️

- [x] crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs #17802 
- [x] crates/ruff_linter/src/rules/pylint/rules/modified_iterating_set.rs #17824 
- [x] crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs #17825 
- [x] crates/ruff_linter/src/rules/pylint/rules/sys_exit_alias.rs #17826 
- [x] crates/ruff_linter/src/rules/pylint/rules/nested_min_max.rs #17878 
- [x] crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs #17932
- [ ] crates/ruff_linter/src/rules/pylint/rules/repeated_equality_comparison.rs #18415

---

_Label `good first issue` removed by @dylwil3 on 2025-05-06 13:25_

---

_Label `help wanted` removed by @dylwil3 on 2025-05-06 13:25_

---

_Comment by @VascoSch92 on 2025-05-13 20:36_

I can work on the `flake8_simplify` rules:

- [x] `crates/ruff_linter/src/rules/flake8_simplify/rules/needless_bool.rs` #18086
- [x] `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_with.rs` #18208 
- [ ] `crates/ruff_linter/src/rules/flake8_simplify/rules/collapsible_if.rs`
- [ ] `crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_dict_get.rs`
- [ ] `crates/ruff_linter/src/rules/flake8_simplify/rules/suppressible_exception.rs`
- [ ] `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_bool_op.rs`
- [x] `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs` #18099 
- [x] `crates/ruff_linter/src/rules/flake8_simplify/rules/reimplemented_builtin.rs` #18114 
- [x] `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_ifexp.rs` #18100 
- [ ] `crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_if_exp.rs`


---

_Comment by @VascoSch92 on 2025-05-18 20:38_

A couple of remarks:

### Rule [SIM105](https://docs.astral.sh/ruff/rules/suppressible-exception/)

I think the fix should be considered safe (when there is one). Because of the following two points:
1. in the Python documentation is explicitly mentioned that the fix is equivalent to the error ([here](https://docs.python.org/3/library/contextlib.html#contextlib.suppress))
2. if there is a comment in the range of the fix, the rule doesn't provide a fix.

[Wait for #18209 to be completed]

### Rule [SIM117](https://docs.astral.sh/ruff/rules/multiple-with-statements/)

As the rule SIM105

1. The violation and the fix are semantically equivalent ([ref](https://docs.python.org/3/reference/compound_stmts.html#the-with-statement))
2. if there is a comment in the range of the fix, the rule doesn't provide a fix.




---

_Comment by @dylwil3 on 2025-05-19 13:16_

@VascoSch92 I think I agree with both of these remarks. Would you like to make a PR that makes these fixes safe under preview?

---

_Comment by @VascoSch92 on 2025-05-19 13:21_

> @VascoSch92 I think I agree with both of these remarks. Would you like to make a PR that makes these fixes safe under preview?

Yes no problem

In this case, should I add a fix safety section too?

---

_Comment by @VascoSch92 on 2025-05-21 06:38_

### Rule [SIM102](https://docs.astral.sh/ruff/rules/collapsible-if/)

I think the fix could be safe in preview mode. For instance, the fix is just applied if there are no comments in the edit range.

### Rule [SIM108](https://docs.astral.sh/ruff/rules/if-else-block-instead-of-if-exp/)

Could also be safe. For instance, the fix is just applied if there are no comments in the edit range.

### Rule [SIM401](https://docs.astral.sh/ruff/rules/if-else-block-instead-of-dict-get/)

The fix of the rule seems to be safe for the following reasons:
- the fix is proposed if and only if it is possible to assert that we are working with a dictionary
- if `dict` is shadowed somewhere in the script, the fix is not proposed
- the fix is not proposed if there are comments in the edit range.


**_Edit_**: @dylwil3 what do you think? :-) 

---

_Comment by @MeGaGiGaGon on 2025-06-19 19:12_

Here's an up-to-date list of ones that are finished, partially done, in progress, and still need done:

# Current status: 41 finished, 3 in progress, 32 still need done

## Finished:

<details>

|Group|File|Fixing PR|
|--|--|--|
|fastapi|[fastapi_non_annotated_dependency.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs)|#18940|
|flake8_async|[long_sleep_not_forever.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_async/rules/long_sleep_not_forever.rs)|#17497|
|flake8_bugbear|[mutable_argument_default.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs)|#17652|
|flake8_logging|[direct_logger_instantiation.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_logging/rules/direct_logger_instantiation.rs)|#18841|
|flake8_logging|[invalid_get_logger_argument.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_logging/rules/invalid_get_logger_argument.rs)|#18840|
|flake8_pie|[duplicate_class_field_definition.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_pie/rules/duplicate_class_field_definition.rs)|#18802|
|flake8_raise|[unnecessary_paren_on_raise_exception.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_raise/rules/unnecessary_paren_on_raise_exception.rs)|#18788|
|flake8_simplify|[ast_expr.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs)|#18099|
|flake8_simplify|[ast_ifexp.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/ast_ifexp.rs)|#18100|
|flake8_simplify|[ast_with.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/ast_with.rs)|#18208|
|flake8_simplify|[needless_bool.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/needless_bool.rs)|#18086|
|flake8_simplify|[reimplemented_builtin.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/reimplemented_builtin.rs)|#18114|
|flake8_use_pathlib|[path_constructor_current_directory.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_use_pathlib/rules/path_constructor_current_directory.rs)|#18837|
|flynt|[static_join_to_fstring.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs)|#17496|
|pycodestyle|[trailing_whitespace.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pycodestyle/rules/trailing_whitespace.rs)|#18800|
|pydocstyle|[one_liner.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pydocstyle/rules/one_liner.rs)|#17502|
|pyflakes|[repeated_keys.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyflakes/rules/repeated_keys.rs)|#17440|
|pyflakes|[unused_variable.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyflakes/rules/unused_variable.rs)|#17410|
|pylint|[modified_iterating_set.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pylint/rules/modified_iterating_set.rs)|#17824|
|pylint|[nested_min_max.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pylint/rules/nested_min_max.rs)|#17878|
|pylint|[repeated_equality_comparison.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pylint/rules/repeated_equality_comparison.rs)|#18415|
|pylint|[sys_exit_alias.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pylint/rules/sys_exit_alias.rs)|#17826|
|pylint|[unnecessary_dunder_call.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs)|#17825|
|pylint|[unspecified_encoding.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs)|#17932|
|pylint|[useless_import_alias.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs)|#17802|
|pyupgrade|[format_literals.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyupgrade/rules/format_literals.rs)|#17443|
|pyupgrade|[non_pep695_type_alias.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_type_alias.rs)|#15840|
|pyupgrade|[outdated_version_block.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyupgrade/rules/outdated_version_block.rs)|#17444|
|pyupgrade|[replace_stdout_stderr.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyupgrade/rules/replace_stdout_stderr.rs)|#17441|
|pyupgrade|[super_call_with_parameters.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs)|#17441|
|pyupgrade|[unnecessary_future_import.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_future_import.rs)|#18838|
|pyupgrade|[useless_object_inheritance.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyupgrade/rules/useless_object_inheritance.rs)|#18853|
|refurb|[for_loop_writes.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs)|#18842|
|ruff|[collection_literal_concatenation.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/collection_literal_concatenation.rs)|#17484|
|ruff|[implicit_optional.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/implicit_optional.rs)|#17759|
|ruff|[invalid_formatter_suppression_comment.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/invalid_formatter_suppression_comment.rs)|#17722|
|ruff|[missing_fstring_syntax.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs)|#17485|
|ruff|[post_init_default.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs)|#17760|
|ruff|[quadratic_list_summation.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/quadratic_list_summation.rs)|#17480|
|ruff|[unnecessary_round.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/unnecessary_round.rs)|#17483|
|ruff|[zip_instead_of_pairwise.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/zip_instead_of_pairwise.rs)|#17755|
|pyupgrade|[unnecessary_future_import.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_builtin_import.rs)|#17490|
|refurb|[print_empty_string.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/refurb/rules/print_empty_string.rs)|#17499|

</details>

## PR in progress:

<details>

|Group|File|Fixing PR|
|--|--|--|
|flake8_errmsg|[string_in_exception.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_errmsg/rules/string_in_exception.rs)|#17493|
|flake8_tidy_imports|[relative_imports.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_tidy_imports/rules/relative_imports.rs)|#17491|
|refurb|[check_and_remove_from_set.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/refurb/rules/check_and_remove_from_set.rs)|#20227|

</details>

## Partially done:
flake8_return [function.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_return/rules/function.rs) RET501 was fixed in #18780, RET503 and RET504 still need done

## Still needs done:

<details>

|Group|File|
|--|--|
|fastapi|[fastapi_redundant_response_model.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/fastapi/rules/fastapi_redundant_response_model.rs)|
|flake8_annotations|[definition.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_annotations/rules/definition.rs)|
|flake8_bugbear|[unused_loop_control_variable.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_bugbear/rules/unused_loop_control_variable.rs)|
|flake8_import_conventions|[unconventional_import_alias.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_import_conventions/rules/unconventional_import_alias.rs)|
|flake8_pyi|[docstring_in_stubs.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_pyi/rules/docstring_in_stubs.rs)|
|flake8_pytest_style|[assertion.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_pytest_style/rules/assertion.rs)|
|flake8_pytest_style|[fixture.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs)|
|flake8_pytest_style|[marks.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_pytest_style/rules/marks.rs)|
|flake8_pytest_style|[parametrize.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs)|
|flake8_simplify|[ast_bool_op.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/ast_bool_op.rs)|
|flake8_simplify|[collapsible_if.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/collapsible_if.rs)|
|flake8_simplify|[if_else_block_instead_of_dict_get.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_dict_get.rs)|
|flake8_simplify|[if_else_block_instead_of_if_exp.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_if_exp.rs)|
|flake8_simplify|[suppressible_exception.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_simplify/rules/suppressible_exception.rs)|
|flake8_type_checking|[runtime_import_in_type_checking_block.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs)|
|flake8_type_checking|[typing_only_runtime_import.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs)|
|numpy|[numpy_2_0_deprecation.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs)|
|pandas_vet|[inplace_argument.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pandas_vet/rules/inplace_argument.rs)|
|perflint|[manual_dict_comprehension.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs)|
|perflint|[manual_list_comprehension.rs ](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs )|
|pycodestyle|[lambda_assignment.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pycodestyle/rules/lambda_assignment.rs)|
|pydocstyle|[backslashes.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pydocstyle/rules/backslashes.rs)|
|pydocstyle|[ends_with_period.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pydocstyle/rules/ends_with_period.rs)|
|pydocstyle|[ends_with_punctuation.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pydocstyle/rules/ends_with_punctuation.rs)|
|pyupgrade|[use_pep604_isinstance.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyupgrade/rules/use_pep604_isinstance.rs) (Was deprecated in #16681)|
|pyupgrade|[useless_class_metaclass_type.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyupgrade/rules/useless_class_metaclass_type.rs)|
|refurb|[delete_full_slice.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/refurb/rules/delete_full_slice.rs)|
|refurb|[implicit_cwd.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/refurb/rules/implicit_cwd.rs)|
|refurb|[repeated_append.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/refurb/rules/repeated_append.rs)|
|refurb|[unnecessary_enumerate.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/refurb/rules/unnecessary_enumerate.rs)|
|ruff|[legacy_form_pytest_raises.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/legacy_form_pytest_raises.rs)|
|ruff|[unused_unpacked_variable.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/unused_unpacked_variable.rs)|

</details>

---

_Comment by @VascoSch92 on 2025-08-30 12:29_

Hey, 
@dylwil3 and @ntBre what is the status here? Should we give him the final shot? :-)

@MeGaGiGaGon is your list above up to date?

---

_Comment by @MeGaGiGaGon on 2025-08-30 17:42_

I just double checked, it should be fully up-to-date (the list in question is here https://github.com/astral-sh/ruff/issues/15584#issuecomment-2988951082 )

---

_Comment by @ntBre on 2025-09-11 13:53_

I was just looking at RUF059, which I didn't realize had an undocumented unsafe fix before the stabilization in 0.13.0. It's tempting to reuse the `## Fix safety` section from the closely related F841:

> This rule's fix is marked as unsafe because removing an unused variable assignment may delete comments that are attached to the assignment.

But I don't think the fix for RUF059 can actually delete a comment, at least I couldn't get it to in my [tests](https://play.ruff.rs/72bf5743-fd38-4efb-98bc-a5ee48ead8e0). But there's also https://github.com/astral-sh/ruff/issues/20339, so I'm not sure it should become a safe fix. Just wanted to record my findings since I didn't end up opening a PR

---
