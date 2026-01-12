```yaml
number: 21440
title: "[`flake8-use-pathlib`] Mark fixes unsafe for return type changes (`PTH104`, `PTH105`, `PTH109`, `PTH115`)"
type: pull_request
state: merged
author: danparizher
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: fix-21431
created_at: 2025-11-13T22:20:16Z
updated_at: 2025-12-01T22:36:41Z
url: https://github.com/astral-sh/ruff/pull/21440
synced_at: 2026-01-12T15:57:24Z
```

# [`flake8-use-pathlib`] Mark fixes unsafe for return type changes (`PTH104`, `PTH105`, `PTH109`, `PTH115`)

---

_@danparizher_

## Summary

Marks fixes as unsafe when they change return types (`None` → `Path`, `str`/`bytes` → `Path`, `str` → `Path`), except when the call is a top-level expression.

Fixes #21431.

## Problem

Fixes for `os.rename`, `os.replace`, `os.getcwd`/`os.getcwdb`, and `os.readlink` were marked safe despite changing return types, which can break code that uses the return value.

## Approach

Added `is_top_level_expression_call` helper to detect when a call is a top-level expression (return value unused). Updated `check_os_pathlib_two_arg_calls` and `check_os_pathlib_single_arg_calls` to mark fixes as unsafe unless the call is a top-level expression. Updated PTH109 to use the helper for applicability determination.

## Test Plan

Updated snapshots for `preview_full_name.py`, `preview_import_as.py`, `preview_import_from.py`, and `preview_import_from_as.py` to reflect unsafe markers.

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 22:31_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -38 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -22 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/32afc282eab817ae801772928e2777d9c8dfb996/airflow-core/src/airflow/cli/cli_config.py#L179'>airflow-core/src/airflow/cli/cli_config.py:179:40:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/32afc282eab817ae801772928e2777d9c8dfb996/airflow-core/src/airflow/cli/cli_config.py#L179'>airflow-core/src/airflow/cli/cli_config.py:179:40:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/32afc282eab817ae801772928e2777d9c8dfb996/airflow-core/tests/unit/utils/log/test_file_processor_handler.py#L44'>airflow-core/tests/unit/utils/log/test_file_processor_handler.py:44:33:</a> PTH115 [*] `os.readlink()` should be replaced by `Path.readlink()`
+ <a href='https://github.com/apache/airflow/blob/32afc282eab817ae801772928e2777d9c8dfb996/airflow-core/tests/unit/utils/log/test_file_processor_handler.py#L44'>airflow-core/tests/unit/utils/log/test_file_processor_handler.py:44:33:</a> PTH115 `os.readlink()` should be replaced by `Path.readlink()`
- <a href='https://github.com/apache/airflow/blob/32afc282eab817ae801772928e2777d9c8dfb996/airflow-core/tests/unit/utils/log/test_file_processor_handler.py#L58'>airflow-core/tests/unit/utils/log/test_file_processor_handler.py:58:33:</a> PTH115 [*] `os.readlink()` should be replaced by `Path.readlink()`
+ <a href='https://github.com/apache/airflow/blob/32afc282eab817ae801772928e2777d9c8dfb996/airflow-core/tests/unit/utils/log/test_file_processor_handler.py#L58'>airflow-core/tests/unit/utils/log/test_file_processor_handler.py:58:33:</a> PTH115 `os.readlink()` should be replaced by `Path.readlink()`
- <a href='https://github.com/apache/airflow/blob/32afc282eab817ae801772928e2777d9c8dfb996/airflow-core/tests/unit/utils/test_cli_util.py#L189'>airflow-core/tests/unit/utils/test_cli_util.py:189:38:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/32afc282eab817ae801772928e2777d9c8dfb996/airflow-core/tests/unit/utils/test_cli_util.py#L189'>airflow-core/tests/unit/utils/test_cli_util.py:189:38:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/32afc282eab817ae801772928e2777d9c8dfb996/airflow-core/tests/unit/utils/test_cli_util.py#L194'>airflow-core/tests/unit/utils/test_cli_util.py:194:37:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/32afc282eab817ae801772928e2777d9c8dfb996/airflow-core/tests/unit/utils/test_cli_util.py#L194'>airflow-core/tests/unit/utils/test_cli_util.py:194:37:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/32afc282eab817ae801772928e2777d9c8dfb996/airflow-ctl-tests/tests/airflowctl_tests/conftest.py#L114'>airflow-ctl-tests/tests/airflowctl_tests/conftest.py:114:47:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/32afc282eab817ae801772928e2777d9c8dfb996/airflow-ctl-tests/tests/airflowctl_tests/conftest.py#L114'>airflow-ctl-tests/tests/airflowctl_tests/conftest.py:114:47:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/32afc282eab817ae801772928e2777d9c8dfb996/dev/breeze/src/airflow_breeze/utils/docs_publisher.py#L87'>dev/breeze/src/airflow_breeze/utils/docs_publisher.py:87:61:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/32afc282eab817ae801772928e2777d9c8dfb996/dev/breeze/src/airflow_breeze/utils/docs_publisher.py#L87'>dev/breeze/src/airflow_breeze/utils/docs_publisher.py:87:61:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/32afc282eab817ae801772928e2777d9c8dfb996/dev/breeze/src/airflow_breeze/utils/run_utils.py#L132'>dev/breeze/src/airflow_breeze/utils/run_utils.py:132:41:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/32afc282eab817ae801772928e2777d9c8dfb996/dev/breeze/src/airflow_breeze/utils/run_utils.py#L132'>dev/breeze/src/airflow_breeze/utils/run_utils.py:132:41:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/32afc282eab817ae801772928e2777d9c8dfb996/providers/elasticsearch/tests/unit/elasticsearch/log/test_es_task_handler.py#L893'>providers/elasticsearch/tests/unit/elasticsearch/log/test_es_task_handler.py:893:48:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/32afc282eab817ae801772928e2777d9c8dfb996/providers/elasticsearch/tests/unit/elasticsearch/log/test_es_task_handler.py#L893'>providers/elasticsearch/tests/unit/elasticsearch/log/test_es_task_handler.py:893:48:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/32afc282eab817ae801772928e2777d9c8dfb996/scripts/ci/prek/common_prek_utils.py#L85'>scripts/ci/prek/common_prek_utils.py:85:29:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
... 4 additional changes omitted for rule PTH109
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +0 -14 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/system.py#L57'>release/system.py:57:50:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/system.py#L57'>release/system.py:57:50:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/system.py#L61'>release/system.py:61:34:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/system.py#L61'>release/system.py:61:34:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/export.py#L527'>src/bokeh/io/export.py:527:76:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/export.py#L527'>src/bokeh/io/export.py:527:76:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/util.py#L78'>src/bokeh/io/util.py:78:36:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/util.py#L78'>src/bokeh/io/util.py:78:36:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/util.py#L48'>src/bokeh/sphinxext/util.py:48:22:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/util.py#L48'>src/bokeh/sphinxext/util.py:48:22:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_code_runner.py#L159'>tests/unit/bokeh/application/handlers/test_code_runner.py:159:16:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_code_runner.py#L159'>tests/unit/bokeh/application/handlers/test_code_runner.py:159:16:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/util/test_browser.py#L98'>tests/unit/bokeh/util/test_browser.py:98:63:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/util/test_browser.py#L98'>tests/unit/bokeh/util/test_browser.py:98:63:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/f3b0c59ba50304148283863c49780e1f25306896/scripts/lib/clean_emoji_cache.py#L37'>scripts/lib/clean_emoji_cache.py:37:43:</a> PTH115 [*] `os.readlink()` should be replaced by `Path.readlink()`
+ <a href='https://github.com/zulip/zulip/blob/f3b0c59ba50304148283863c49780e1f25306896/scripts/lib/clean_emoji_cache.py#L37'>scripts/lib/clean_emoji_cache.py:37:43:</a> PTH115 `os.readlink()` should be replaced by `Path.readlink()`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PTH109 | 32 | 0 | 0 | 0 | 32 |
| PTH115 | 6 | 0 | 0 | 0 | 6 |

</p>
</details>





---

_Label `fixes` added by @ntBre on 2025-11-14 15:23_

---

_Label `preview` added by @ntBre on 2025-11-14 15:23_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_getcwd.rs`:98 on 2025-11-14 19:02_

I think I would slightly prefer something like:


```suggestion
// Unsafe when the fix would delete comments or change a used return value
let applicability = if checker.comment_ranges().intersects(range) 
  || !is_top_level_expression_call(checker, call) {
                Applicability::Unsafe
            } else {
                Applicability::Safe
};
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_getcwd.rs`:43 on 2025-11-14 19:04_

I think we could simplify this slightly to something like:

```suggestion
/// Additionally, the fix is marked as unsafe when the return value is used because the type changes
/// from `str` or `bytes` to a `Path` object.
```

I think something similar would work for the other documentation updates too.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__preview_full_name.py.snap`:320 on 2025-11-14 19:07_

This doesn't look right. Should `isfile` be affected by these changes? I think both versions return a bool.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__preview_full_name.py.snap`:348 on 2025-11-14 19:08_

I think this one also shouldn't change.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__preview_import_as.py.snap`:236 on 2025-11-14 19:08_

Another one that shouldn't change, from what I can tell.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:243 on 2025-11-14 19:11_

Assuming that `call` is the `SemanticModel::current_expression`, I think this function could just be:


```suggestion
    checker.semantic().current_expression_parent().is_none()
```

---

_@ntBre requested changes on 2025-11-14 19:13_

I made a couple of suggestions for simplification, but we also need to verify that this change is only applied to the correct rules. I flagged a few cases inline where rules not mentioned in #21431 were changed unnecessarily, but I didn't check them exhaustively.

---

_Comment by @danparizher on 2025-11-14 20:59_

Thanks for the feedback! I had thought that the same concepts applied to a few other rules, but it appears not after taking a closer look.

---

_Review requested from @ntBre by @danparizher on 2025-11-14 20:59_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:107 on 2025-11-18 21:51_

I don't think we should do this check here. I checked all of the references for `check_os_pathlib_single_arg_calls`, and it's only used by PTH115, of the rules we're trying to fix in this PR.

I think we should just do the `is_top_level_expression_call` check in PTH115 itself.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:194 on 2025-11-18 21:52_

As above, this helper also affects rules outside of PTH{104,105,109,115}. We should narrow the check to the affected rules and pass that applicability into the helper.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:229 on 2025-11-18 21:52_

If `_call` is not used, we should remove it from the signature.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_readlink.rs`:42 on 2025-11-18 21:58_

nit: I think this one returns `str` or `bytes`. The annotation in the ty playground hover says [`AnyStr`](https://docs.python.org/3/library/typing.html#typing.AnyStr): https://play.ty.dev/f5f68ba2-0762-4d18-b28c-5ebef25bf9be



---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_dirname.rs`:45 on 2025-11-18 22:01_

I think this is also bytes or str (`AnyStr`).

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_abspath.rs`:48 on 2025-11-18 22:01_

This one is also `AnyStr` in ty.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_getcwd.rs`:93 on 2025-11-18 22:03_

This looks good, this is the pattern I'd follow for the other rules, if possible.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_expanduser.rs`:44 on 2025-11-18 22:03_

This is also `AnyStr` in ty

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_isfile.rs`:8 on 2025-11-18 22:04_

nit: I would group these ruff imports with the others, separate from the `crate::` imports.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_islink.rs`:77 on 2025-11-18 22:05_

Hmm, are there any cases left where we pass `None`? I think we could just make this a required argument. `None` and `Safe` were being treated as the same anyway, I think, so I don't see any benefit to using the Option.

---

_@ntBre requested changes on 2025-11-18 22:08_

Thanks! This is looking better, but I had a few more suggestions.

I tried to flag all of the places the docs should say "str or bytes," but I started using [`typing.AnyStr`](https://docs.python.org/3/library/typing.html#typing.AnyStr) as a shorthand since that's what ty shows. It's technically a type variable that depends on the input, but I think it's fine just to say "str or bytes" in each of those cases.

---

_@danparizher reviewed on 2025-11-20 00:17_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:194 on 2025-11-20 00:17_

Just a note on this - to have applicability passed through the helper, I had to add `#[allow(clippy::too_many_arguments)]`.

Let me know if that's fine or if you'd rather have it refactored.

---

_Review requested from @ntBre by @danparizher on 2025-11-20 00:24_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:229 on 2025-11-21 22:10_

This is not resolved.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:142 on 2025-11-21 22:12_

Let's use `expect` instead of `allow`. That will remind us to remove it if we ever remove arguments.


```suggestion
#[expect(clippy::too_many_arguments)]
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:151 on 2025-11-21 22:15_

I still don't see why we're using an `Option` here (https://github.com/astral-sh/ruff/pull/21440#discussion_r2539766325). If we're doing `unwrap_or(Safe)`, why don't we just have the callers pass in `Safe` to begin with?

That also applies to the single-argument helper, though I know that was pre-existing.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_exists.rs`:8 on 2025-11-21 22:20_

```suggestion
use ruff_diagnostics::Applicability;
use ruff_macros::{ViolationMetadata, derive_message_formats};
use ruff_python_ast::ExprCall;

use crate::checkers::ast::Checker;
use crate::preview::is_fix_os_path_exists_enabled;
use crate::rules::flake8_use_pathlib::helpers::check_os_pathlib_single_arg_calls;
use crate::{FixAvailability, Violation};
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_readlink.rs`:11 on 2025-11-21 22:21_

```suggestion
use ruff_diagnostics::Applicability;
use ruff_macros::{ViolationMetadata, derive_message_formats};
use ruff_python_ast::{ExprCall, PythonVersion};

use crate::checkers::ast::Checker;
use crate::preview::is_fix_os_readlink_enabled;
use crate::rules::flake8_use_pathlib::helpers::{
check_os_pathlib_single_arg_calls, is_keyword_only_argument_non_default,
    is_top_level_expression_call,
};
use crate::{FixAvailability, Violation};
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_replace.rs`:97 on 2025-11-21 22:22_

We don't need to check if the fix is enabled here. We already pass it into the helper.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:105 on 2025-11-21 22:28_

See my other comment about the `Option`, but I think we should also do something more like this to account for the possibility of `DisplayOnly` fixes, which shouldn't be "elevated" to unsafe if there are comments. This is unlikely to happen in these rules, but it's not really more code to handle it properly.

```suggestion
        let applicability = match applicability {
            Applicability::DisplayOnly => Applicability::DisplayOnly,
            _ if checker.comment_ranges().intersects(range) => Applicability::Unsafe,
            _ => applicability,
        };
```

---

_@ntBre requested changes on 2025-11-21 22:29_

---

_Review requested from @ntBre by @danparizher on 2025-11-29 01:18_

---

_@ntBre approved on 2025-12-01 20:05_

Thanks! I just fixed one more round of tiny nits. Once the ecosystem check runs, I'll merge.

---

_Merged by @ntBre on 2025-12-01 20:26_

---

_Closed by @ntBre on 2025-12-01 20:26_

---

_Branch deleted on 2025-12-01 22:36_

---
