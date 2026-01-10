```yaml
number: 18763
title: "[`flake8-use-pathlib`] Add autofix for `PTH202`"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: feat/pth202-autofix
created_at: 2025-06-18T17:47:00Z
updated_at: 2025-06-24T18:18:56Z
url: https://github.com/astral-sh/ruff/pull/18763
synced_at: 2026-01-10T18:39:08Z
```

# [`flake8-use-pathlib`] Add autofix for `PTH202`

---

_Pull request opened by @chirizxc on 2025-06-18 17:47_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
/closes #2331
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
update snapshots
<!-- How was it tested? -->


---

_Review requested from @ntBre by @ntBre on 2025-06-18 17:50_

---

_Label `fixes` added by @ntBre on 2025-06-18 17:50_

---

_Comment by @chirizxc on 2025-06-18 17:51_

this not the final version yet, and i still have some questions

---

_Comment by @chirizxc on 2025-06-18 17:56_

should we remove [ `import os`, `import import os.path`, `from os.path import getsize` ] if it is not needed after the fix?
```python
import os

x = os.path.getsize(filename="filename")
y = os.path.getsize(filename=b"filename")
z = os.path.getsize(filename=__file__)
```
=>
```python
import os # unused
from pathlib import Path # we also have to import it if it wasn't there before

x = Path("filename").stat().st_size
y = Path(b"filename").stat().st_size
z = Path(__file__).stat().st_size
```

---

_Comment by @ntBre on 2025-06-18 18:00_

I don't think you need to remove the imports, you can defer to [unused-import (F401)](https://docs.astral.sh/ruff/rules/unused-import/#unused-import-f401) for that, but you do need to add the `Path` import if it's required for the fix.

Let me know if you have more questions or when it's ready for review!

---

_Comment by @chirizxc on 2025-06-18 18:01_

as well as, for example
```python
os.path.getsize(Path("filename"))
```
=>
```python 
Path(Path("filename")).stat().st_size
```

---

_Comment by @chirizxc on 2025-06-18 18:03_

The call to Path with parentheses here seems redundant, but I don't think we can consider it unnecessary since it might have additional methods like `.parents[3]` or `.resolve()`

---

_Comment by @github-actions[bot] on 2025-06-18 18:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +20 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +8 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/d378410cdd9df2b5324df3b5b37f6b3afd16ed81/airflow-core/tests/unit/utils/test_log_handlers.py#L375'>airflow-core/tests/unit/utils/test_log_handlers.py:375:29:</a> PTH202 [*] `os.path.getsize` should be replaced by `Path.stat().st_size`
- <a href='https://github.com/apache/airflow/blob/d378410cdd9df2b5324df3b5b37f6b3afd16ed81/airflow-core/tests/unit/utils/test_log_handlers.py#L375'>airflow-core/tests/unit/utils/test_log_handlers.py:375:29:</a> PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`
+ <a href='https://github.com/apache/airflow/blob/d378410cdd9df2b5324df3b5b37f6b3afd16ed81/airflow-core/tests/unit/utils/test_log_handlers.py#L376'>airflow-core/tests/unit/utils/test_log_handlers.py:376:30:</a> PTH202 [*] `os.path.getsize` should be replaced by `Path.stat().st_size`
- <a href='https://github.com/apache/airflow/blob/d378410cdd9df2b5324df3b5b37f6b3afd16ed81/airflow-core/tests/unit/utils/test_log_handlers.py#L376'>airflow-core/tests/unit/utils/test_log_handlers.py:376:30:</a> PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`
+ <a href='https://github.com/apache/airflow/blob/d378410cdd9df2b5324df3b5b37f6b3afd16ed81/dev/breeze/src/airflow_breeze/utils/parallel.py#L308'>dev/breeze/src/airflow_breeze/utils/parallel.py:308:24:</a> PTH202 [*] `os.path.getsize` should be replaced by `Path.stat().st_size`
- <a href='https://github.com/apache/airflow/blob/d378410cdd9df2b5324df3b5b37f6b3afd16ed81/dev/breeze/src/airflow_breeze/utils/parallel.py#L308'>dev/breeze/src/airflow_breeze/utils/parallel.py:308:24:</a> PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`
+ <a href='https://github.com/apache/airflow/blob/d378410cdd9df2b5324df3b5b37f6b3afd16ed81/providers/amazon/src/airflow/providers/amazon/aws/transfers/dynamodb_to_s3.py#L244'>providers/amazon/src/airflow/providers/amazon/aws/transfers/dynamodb_to_s3.py:244:16:</a> PTH202 [*] `os.path.getsize` should be replaced by `Path.stat().st_size`
- <a href='https://github.com/apache/airflow/blob/d378410cdd9df2b5324df3b5b37f6b3afd16ed81/providers/amazon/src/airflow/providers/amazon/aws/transfers/dynamodb_to_s3.py#L244'>providers/amazon/src/airflow/providers/amazon/aws/transfers/dynamodb_to_s3.py:244:16:</a> PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +12 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/5170a4ad28847631caaf4bf062260cdf1cad6b13/scripts/setup/generate_secrets.py#L65'>scripts/setup/generate_secrets.py:65:47:</a> PTH202 [*] `os.path.getsize` should be replaced by `Path.stat().st_size`
- <a href='https://github.com/zulip/zulip/blob/5170a4ad28847631caaf4bf062260cdf1cad6b13/scripts/setup/generate_secrets.py#L65'>scripts/setup/generate_secrets.py:65:47:</a> PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`
+ <a href='https://github.com/zulip/zulip/blob/5170a4ad28847631caaf4bf062260cdf1cad6b13/tools/lib/test_server.py#L64'>tools/lib/test_server.py:64:45:</a> PTH202 [*] `os.path.getsize` should be replaced by `Path.stat().st_size`
- <a href='https://github.com/zulip/zulip/blob/5170a4ad28847631caaf4bf062260cdf1cad6b13/tools/lib/test_server.py#L64'>tools/lib/test_server.py:64:45:</a> PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`
+ <a href='https://github.com/zulip/zulip/blob/5170a4ad28847631caaf4bf062260cdf1cad6b13/zerver/data_import/mattermost.py#L361'>zerver/data_import/mattermost.py:361:21:</a> PTH202 [*] `os.path.getsize` should be replaced by `Path.stat().st_size`
- <a href='https://github.com/zulip/zulip/blob/5170a4ad28847631caaf4bf062260cdf1cad6b13/zerver/data_import/mattermost.py#L361'>zerver/data_import/mattermost.py:361:21:</a> PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`
+ <a href='https://github.com/zulip/zulip/blob/5170a4ad28847631caaf4bf062260cdf1cad6b13/zerver/data_import/slack.py#L1550'>zerver/data_import/slack.py:1550:70:</a> PTH202 [*] `os.path.getsize` should be replaced by `Path.stat().st_size`
- <a href='https://github.com/zulip/zulip/blob/5170a4ad28847631caaf4bf062260cdf1cad6b13/zerver/data_import/slack.py#L1550'>zerver/data_import/slack.py:1550:70:</a> PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`
+ <a href='https://github.com/zulip/zulip/blob/5170a4ad28847631caaf4bf062260cdf1cad6b13/zerver/lib/export.py#L2955'>zerver/lib/export.py:2955:41:</a> PTH202 [*] `os.path.getsize` should be replaced by `Path.stat().st_size`
- <a href='https://github.com/zulip/zulip/blob/5170a4ad28847631caaf4bf062260cdf1cad6b13/zerver/lib/export.py#L2955'>zerver/lib/export.py:2955:41:</a> PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`
+ <a href='https://github.com/zulip/zulip/blob/5170a4ad28847631caaf4bf062260cdf1cad6b13/zerver/lib/upload/local.py#L110'>zerver/lib/upload/local.py:110:45:</a> PTH202 [*] `os.path.getsize` should be replaced by `Path.stat().st_size`
- <a href='https://github.com/zulip/zulip/blob/5170a4ad28847631caaf4bf062260cdf1cad6b13/zerver/lib/upload/local.py#L110'>zerver/lib/upload/local.py:110:45:</a> PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PTH202 | 20 | 0 | 0 | 20 | 0 |

</p>
</details>




---

_Comment by @chirizxc on 2025-06-18 19:38_

as i understand: [`PTH203`](https://docs.astral.sh/ruff/rules/os-path-getatime/), [`PTH204`](https://docs.astral.sh/ruff/rules/os-path-getmtime/), [`PTH205`](https://docs.astral.sh/ruff/rules/os-path-getctime/) will be exactly the same in implementation

---

_Comment by @chirizxc on 2025-06-18 19:43_

is it worth splitting pr if the changes are the same as this pr? the question is more to [this one](https://github.com/astral-sh/ruff/issues/18752#issuecomment-2984316166)

---

_Comment by @MichaReiser on 2025-06-19 14:39_

> is it worth splitting pr if the changes are the same as this pr? the question is more to https://github.com/astral-sh/ruff/issues/18752#issuecomment-2984316166

Let's start with one fix. Then the second PR can generalize it and apply it to the other rules.

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1059 on 2025-06-20 13:49_

It doesn't seem to be causing any duplicate diagnostics, but I think you should also remove this line in `replaceable_by_pathlib`:

https://github.com/astral-sh/ruff/blob/e8b7fdf5a8ede8cf517bb41646fe04b77598cbd0/crates/ruff_linter/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs#L197-L198

Alternatively you could try to integrate the fix generation inside of `replaceable_by_pathlib`, likely by calling out to a function similar to the one you added.

I think we need to look closely at the ecosystem results because I wouldn't expect more violations to be found when just adding a fix. It looks like the rule implementation itself has changed.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_getsize.rs`:82 on 2025-06-20 13:54_

I think I would prefer something more like:

```suggestion
    if call.arguments.len() != 1 {
        return;
    }

    let Some(arg) = call.arguments.find_argument_value("filename", 0) else {
        return;
    };
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_getsize.rs`:96 on 2025-06-20 13:56_

This doesn't look right. What are you trying to accomplish here?

I don't think we should be doing a replacement if the import fails, the `get_or_import_symbol` call should be in `try_set_fix` so that any error is logged, and the current `try_set_fix` call isn't doing anything since the contents are always `Ok`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_getsize.rs`:111 on 2025-06-20 13:57_

As above, this `try_set_fix` is unnecessary since there's no error condition, but I think all of these can be combined eventually.

---

_@ntBre requested changes on 2025-06-20 13:58_

Thanks! I have some questions and suggestions here.

---

_@chirizxc reviewed on 2025-06-20 14:13_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1059 on 2025-06-20 14:13_

I think it would be better to separate the rules in this file into individual files. As for the ecosystem results, i think it detects them correctly

---

_@ntBre reviewed on 2025-06-20 14:24_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1059 on 2025-06-20 14:24_

I cloned both airflow and zulip, and these diagnostics are also present on `main`, so this must be a false positive in the ecosystem report.

And I agree that it makes sense to separate the rules into separate files, like most rules. In that case, you can just remove the line in `replaceable_by_pathlib`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH202_PTH202.py.snap`:58 on 2025-06-24 12:51_

Have you looked into how hard it would be to detect this? I think it would just be a `match` on the argument to see if it's a call to `Path`. I don't think this is a deal-breaker for this PR, but it is a bit unfortunate, as you pointed out.

I think if you look just for an `Expr::Call`, it should avoid your concern about additional attributes like `Path(...).resolve()` because those would be `Expr::Attribute`s.

---

_@ntBre approved on 2025-06-24 12:57_

Thanks, this looks good. I often forget this, but based on our [versioning](https://docs.astral.sh/ruff/versioning/#version-changes) policy, adding a safe fix actually needs to be done in preview. Could you add a preview method like this and gate the fix behind that?

https://github.com/astral-sh/ruff/blob/833be2e66a55841aa0c581a28a0a3d66056b3a60/crates/ruff_linter/src/preview.rs#L43-L46

---

_Comment by @chirizxc on 2025-06-24 14:49_

> Thanks, this looks good. I often forget this, but based on our [versioning](https://docs.astral.sh/ruff/versioning/#version-changes) policy, adding a safe fix actually needs to be done in preview. Could you add a preview method like this and gate the fix behind that?
> 
> https://github.com/astral-sh/ruff/blob/833be2e66a55841aa0c581a28a0a3d66056b3a60/crates/ruff_linter/src/preview.rs#L43-L46

should the changes disappear after that in the snapshots?

---

_Comment by @chirizxc on 2025-06-24 14:51_

> > Thanks, this looks good. I often forget this, but based on our [versioning](https://docs.astral.sh/ruff/versioning/#version-changes) policy, adding a safe fix actually needs to be done in preview. Could you add a preview method like this and gate the fix behind that?
> > https://github.com/astral-sh/ruff/blob/833be2e66a55841aa0c581a28a0a3d66056b3a60/crates/ruff_linter/src/preview.rs#L43-L46
> 
> should the changes disappear after that in the spanshots?

![изображение](https://github.com/user-attachments/assets/53687bc4-bba1-494c-9dac-bad2bdead3b6)
i added `is_fix_os_path_path_getsize_enabled`, but the changes of fix to the snapshots are gone

---

_Comment by @ntBre on 2025-06-24 15:36_

Yes, you'll need to add a version of the PTH202 test that enables preview mode to see the preview snapshot changes.

---

_Comment by @chirizxc on 2025-06-24 16:18_

done

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_getsize.rs`:128 on 2025-06-24 17:55_

```suggestion
fn is_path_call(checker: &Checker, expr: &Expr) -> bool {
    expr.as_call_expr().is_some_and(|expr_call| {
        checker
            .semantic()
            .resolve_qualified_name(&expr_call.func)
            .is_some_and(|name| matches!(name.segments(), ["pathlib", "Path"]))
    })
}
```

---

_@ntBre approved on 2025-06-24 17:55_

Thanks!

---

_Label `preview` added by @ntBre on 2025-06-24 17:55_

---

_Renamed from "[`flake8-use-pathlib`] add autofix for `PTH202`" to "[`flake8-use-pathlib`] Add autofix for `PTH202`" by @ntBre on 2025-06-24 17:55_

---

_Merged by @ntBre on 2025-06-24 17:58_

---

_Closed by @ntBre on 2025-06-24 17:58_

---

_Branch deleted on 2025-06-24 18:18_

---
