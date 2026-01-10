---
number: 1915
title: tests fail on windows due to platform line endings
type: issue
state: closed
author: sbrugman
labels:
  - internal
assignees: []
created_at: 2023-01-16T13:04:06Z
updated_at: 2023-01-26T00:53:23Z
url: https://github.com/astral-sh/ruff/issues/1915
synced_at: 2026-01-10T01:22:40Z
---

# tests fail on windows due to platform line endings

---

_Issue opened by @sbrugman on 2023-01-16 13:04_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->
While developing on a Windows machine, tests that explicitly test for line endings fail when running `cargo test`.

### To reproduce:

- Checkout the repo on a windows machine
- Run `cargo test`

### Output

```
failures:
    flake8_comprehensions::tests::rules::c400
    flake8_comprehensions::tests::rules::c401
    flake8_comprehensions::tests::rules::c402
    flake8_comprehensions::tests::rules::c403
    flake8_comprehensions::tests::rules::c409
    isort::tests::combine_as_imports::path_new_combine_as_imports_py_expects
    isort::tests::default::path_new_add_newline_before_comments_py_expects
    isort::tests::default::path_new_combine_as_imports_py_expects
    isort::tests::default::path_new_combine_import_from_py_expects
    isort::tests::default::path_new_comments_py_expects
    isort::tests::default::path_new_deduplicate_imports_py_expects
    isort::tests::default::path_new_fit_line_length_comment_py_expects
    isort::tests::default::path_new_fit_line_length_py_expects
    isort::tests::default::path_new_force_sort_within_sections_py_expects
    isort::tests::default::path_new_force_wrap_aliases_py_expects
    isort::tests::default::path_new_import_from_after_import_py_expects
    isort::tests::default::path_new_inline_comments_py_expects
    isort::tests::default::path_new_insert_empty_lines_py_expects
    isort::tests::default::path_new_insert_empty_lines_pyi_expects
    isort::tests::default::path_new_line_ending_lf_py_expects
    isort::tests::default::path_new_magic_trailing_comma_py_expects
    isort::tests::default::path_new_natural_order_py_expects
    isort::tests::default::path_new_no_wrap_star_py_expects
    isort::tests::default::path_new_order_by_type_py_expects
    isort::tests::default::path_new_order_relative_imports_by_level_py_expects
    isort::tests::default::path_new_preserve_comment_order_py_expects
    isort::tests::default::path_new_preserve_import_star_py_expects
    isort::tests::default::path_new_preserve_indentation_py_expects
    isort::tests::default::path_new_reorder_within_section_py_expects
    isort::tests::default::path_new_separate_first_party_imports_py_expects
    isort::tests::default::path_new_separate_future_imports_py_expects
    isort::tests::default::path_new_separate_local_folder_imports_py_expects
    isort::tests::default::path_new_separate_third_party_imports_py_expects
    isort::tests::default::path_new_skip_py_expects
    isort::tests::default::path_new_sort_similar_imports_py_expects
    isort::tests::force_single_line::path_new_force_single_line_py_expects
    isort::tests::force_sort_within_sections::path_new_force_sort_within_sections_py_expects
    isort::tests::force_wrap_aliases::path_new_force_wrap_aliases_py_expects
    isort::tests::no_split_on_trailing_comma::path_new_magic_trailing_comma_py_expects
    isort::tests::order_by_type::path_new_order_by_type_py_expects
    pydocstyle::tests::rules::d301
    pyflakes::tests::future_annotations
    pyflakes::tests::rules::f401_0
    pyflakes::tests::rules::f401_7
    pyupgrade::tests::rules::up012
    pyupgrade::tests::rules::up013
    pyupgrade::tests::rules::up014
    pyupgrade::tests::rules::up018
    pyupgrade::tests::rules::up022
    pyupgrade::tests::rules::up023
    pyupgrade::tests::rules::up026
    pyupgrade::tests::rules::up027
    pyupgrade::tests::rules::up028_0
    pyupgrade::tests::rules::up030_0
    ruff::tests::ruf100_1
```

Example

```
---- pyupgrade::tests::rules::up030_0 stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Differences ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot file: src\pyupgrade\snapshots\ruff__pyupgrade__tests__UP030_UP030_0.py.snap
Snapshot: UP030_UP030_0.py
Source: C:\Users\[XXX]\Documents\code\ruff:63
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
Expression: diagnostics
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   22    22 │   end_location:
   23    23 │     row: 7
   24    24 │     column: 1
   25    25 │   fix:
   26       │-    content: "\"a {} complicated {} string with {} {}\".format(\n    \"fourth\", \"second\", \"first\", \"third\"\n)"
         26 │+    content: "\"a {} complicated {} string with {} {}\".format(\r\n    \"fourth\", \"second\", \"first\", \"third\"\r\n)"
   27    27 │     location:
   28    28 │       row: 5
   29    29 │       column: 0
   30    30 │     end_location:
┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
  107   107 │   end_location:
  108   108 │     row: 18
  109   109 │     column: 26
  110   110 │   fix:
  111       │-    content: "\"foo {}\" \\\n    \"bar {}\".format(1, 2)"
        111 │+    content: "\"foo {}\" \\\r\n    \"bar {}\".format(1, 2)"
  112   112 │     location:
  113   113 │       row: 17
  114   114 │       column: 4
  115   115 │     end_location:
────────────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

### Suggested solution:

- Snapshot files should expect `\r\n` when development machine is Windows
- CI should include Windows-latest in matrix to detect issues in the future: https://github.com/charliermarsh/ruff/blob/main/.github/workflows/ci.yaml#L66

### Alternatives considered:

Ignore tests that depend on platform-dependent line separators on Windows. 

---

_Comment by @charliermarsh on 2023-01-16 17:22_

Hah, yeah, that's interesting and not something I'd thought of. Insta actually _does_ normalize newlines in this way, but the content that's failing here is embedded in strings in our output.

---

_Label `internal` added by @charliermarsh on 2023-01-16 17:22_

---

_Referenced in [astral-sh/ruff#2033](../../astral-sh/ruff/pulls/2033.md) on 2023-01-20 16:08_

---

_Comment by @charliermarsh on 2023-01-26 00:53_

I believe this is closed by #2033!

---

_Closed by @charliermarsh on 2023-01-26 00:53_

---
