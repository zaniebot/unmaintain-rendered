```yaml
number: 17355
title: "[`airflow`] Apply auto fixes to cases where the names have changed in Airflow 3 (`AIR301`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: apply-auto-import
created_at: 2025-04-11T15:59:47Z
updated_at: 2025-04-23T16:43:41Z
url: https://github.com/astral-sh/ruff/pull/17355
synced_at: 2026-01-10T19:33:02Z
```

# [`airflow`] Apply auto fixes to cases where the names have changed in Airflow 3 (`AIR301`)

---

_Pull request opened by @Lee-W on 2025-04-11 15:59_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Apply auto fixes to cases where the names have changed in Airflow 3

## Test Plan

<!-- How was it tested? -->

Add `AIR301_names_fix.py` and `AIR301_provider_names_fix.py` test fixtures


---

_Comment by @Lee-W on 2025-04-11 16:06_

Hey @ntBre , I found out a `Failed to converge after 10 iterations. This likely indicates a bug in the implementation of the fix. Last diagnostics:` error during testing. but the traceback does not provide much information. Do you know what the cause might be? thanks!

---

_Comment by @ntBre on 2025-04-11 16:12_

> Hey @ntBre , I found out a `Failed to converge after 10 iterations. This likely indicates a bug in the implementation of the fix. Last diagnostics:` error during testing. but the traceback does not provide much information. Do you know what the cause might be? thanks!

That usually points to a loop between two rules, for example one rule might add an import and the other deletes it. Based on the test that's failing, it probably only has one rule (AIR301) enabled, so it would probably have to be a loop within that rule. Do any of the replacements overlap? Something like

```python
import a  # error, import b instead
```

Fixed to

```python
import b  # error, import a instead
```

Fixed to

```python
import a
```

and so on. There could even be more parts in the chain like `a -> b -> c -> a`, which could make it harder to identify, but something like this should be the cause.

---

_Comment by @Lee-W on 2025-04-12 01:47_

> > Hey @ntBre , I found out a `Failed to converge after 10 iterations. This likely indicates a bug in the implementation of the fix. Last diagnostics:` error during testing. but the traceback does not provide much information. Do you know what the cause might be? thanks!
> 
> That usually points to a loop between two rules, for example one rule might add an import and the other deletes it. Based on the test that's failing, it probably only has one rule (AIR301) enabled, so it would probably have to be a loop within that rule. Do any of the replacements overlap? Something like
> 
> ```python
> import a  # error, import b instead
> ```
> 
> Fixed to
> 
> ```python
> import b  # error, import a instead
> ```
> 
> Fixed to
> 
> ```python
> import a
> ```
> 
> and so on. There could even be more parts in the chain like `a -> b -> c -> a`, which could make it harder to identify, but something like this should be the cause.

Hmm... don't think we have such case. but will take a deeper look.

---

_Comment by @dhruvmanila on 2025-04-15 13:45_

I'm not sure how the fix has been implemented but a potential reason this might be happening is there are multiple autofixes (more than the iteration limit) which is trying to edit the same line of import. This could happen when there are multiple references of the same deprecated symbol and each reference has generated an edit to update the import. Now, that edit belongs to the same line so Ruff will only apply a single edit and avoid fixing the rest. Those will then be carried over to the next iteration. I guess this is why the autoimporter logic created an error. But, I'll need to look closely as to why this is happening.

It might be worth printing out some of the variables in the fix loop to see why the linter is not able to apply the fixes in a single loop.

(Posting from phone)

---

_Comment by @Lee-W on 2025-04-15 14:18_

> I'm not sure how the fix has been implemented but a potential reason this might be happening is there are multiple autofixes (more than the iteration limit) which is trying to edit the same line of import. This could happen when there are multiple references of the same deprecated symbol and each reference has generated an edit to update the import. Now, that edit belongs to the same line so Ruff will only apply a single edit and avoid fixing the rest. Those will then be carried over to the next iteration. I guess this is why the autoimporter logic created an error. But, I'll need to look closely as to why this is happening.
> 
> It might be worth printing out some of the variables in the fix loop to see why the linter is not able to apply the fixes in a single loop.
> 
> (Posting from phone)

I've not yet had the bandwidth to come back to this one. 😞 But multiple fix on the same line sounds like a highly likely cause 🤔 

---

_Renamed from "Apply auto import" to "[`airflow`] Apply fixes to cases that names that have been changed in Airflow 3 (`AIR301`)" by @Lee-W on 2025-04-21 14:52_

---

_Renamed from "[`airflow`] Apply fixes to cases that names that have been changed in Airflow 3 (`AIR301`)" to "[`airflow`] Apply fixes to cases that names changed in Airflow 3 (`AIR301`)" by @Lee-W on 2025-04-21 14:57_

---

_Renamed from "[`airflow`] Apply fixes to cases that names changed in Airflow 3 (`AIR301`)" to "[`airflow`] Apply auto fixes to cases where the names have changed in Airflow 3 (`AIR301`)" by @Lee-W on 2025-04-21 14:57_

---

_Marked ready for review by @Lee-W on 2025-04-21 14:57_

---

_Comment by @Lee-W on 2025-04-21 14:59_

Hey, @ntBre I just wrap up this one. Would be nice if you could take a look when you have time. Thanks 🙂 

---

_Comment by @github-actions[bot] on 2025-04-21 15:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +10 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +10 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/c01772f959fd5c73198456e429c5d7823db3fc45/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L39'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:39:32:</a> AIR301 [*] `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c01772f959fd5c73198456e429c5d7823db3fc45/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L39'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:39:32:</a> AIR301 `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/c01772f959fd5c73198456e429c5d7823db3fc45/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L39'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:39:5:</a> AIR301 [*] `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c01772f959fd5c73198456e429c5d7823db3fc45/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L39'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:39:5:</a> AIR301 `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/c01772f959fd5c73198456e429c5d7823db3fc45/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L63'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:63:17:</a> AIR301 [*] `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c01772f959fd5c73198456e429c5d7823db3fc45/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L63'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:63:17:</a> AIR301 `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/c01772f959fd5c73198456e429c5d7823db3fc45/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L67'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:67:17:</a> AIR301 [*] `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c01772f959fd5c73198456e429c5d7823db3fc45/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L67'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:67:17:</a> AIR301 `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c01772f959fd5c73198456e429c5d7823db3fc45/providers/common/compat/src/airflow/providers/common/compat/openlineage/utils/utils.py#L40'>providers/common/compat/src/airflow/providers/common/compat/openlineage/utils/utils.py:40:59:</a> AIR301 `airflow.providers.openlineage.utils.utils.translate_airflow_dataset` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/c01772f959fd5c73198456e429c5d7823db3fc45/providers/openlineage/tests/unit/openlineage/plugins/test_utils.py#L580'>providers/openlineage/tests/unit/openlineage/plugins/test_utils.py:580:21:</a> AIR301 [*] `airflow.timetables.simple.DatasetTriggeredTimetable` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c01772f959fd5c73198456e429c5d7823db3fc45/providers/openlineage/tests/unit/openlineage/plugins/test_utils.py#L580'>providers/openlineage/tests/unit/openlineage/plugins/test_utils.py:580:21:</a> AIR301 `airflow.timetables.simple.DatasetTriggeredTimetable` is removed in Airflow 3.0
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR301 | 11 | 0 | 1 | 10 | 0 |

</p>
</details>




---

_Assigned to @ntBre by @ntBre on 2025-04-21 17:12_

---

_@ntBre approved on 2025-04-21 21:52_

Thanks, looks good to me!

---

_Label `fixes` added by @ntBre on 2025-04-21 21:52_

---

_Label `preview` added by @ntBre on 2025-04-21 21:52_

---

_Comment by @ntBre on 2025-04-21 21:55_

I guess one quick question about the ecosystem check: `airflow.providers.openlineage.utils.utils.translate_airflow_dataset` is no longer being detected/fixed. Is that expected?

Otherwise the ecosystem check looks good, it shows new fixes for the other matches.

---

_Comment by @Lee-W on 2025-04-22 06:15_

> I guess one quick question about the ecosystem check: `airflow.providers.openlineage.utils.utils.translate_airflow_dataset` is no longer being detected/fixed. Is that expected?
> 
> Otherwise the ecosystem check looks good, it shows new fixes for the other matches.

I think it's fixed here? https://github.com/astral-sh/ruff/pull/17355/files#diff-493d19bbac615454a597e22069204d09be7c4e125ce6494d9cd85289278b141fR231

or are you referring to something else

---

_Comment by @ntBre on 2025-04-22 20:06_

I'm talking about this entry from the ecosystem check: https://github.com/apache/airflow/blob/689608d7423cebf7c72d0e0c0081eceeeac51b3a/providers/common/compat/src/airflow/providers/common/compat/openlineage/utils/utils.py#L40

All of the other entries have one new diagnostic (+) and one removed diagnostic (-), but this one shows only a removed diagnostic:

```text
+ [providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:39:32:](https://github.com/apache/airflow/blob/689608d7423cebf7c72d0e0c0081eceeeac51b3a/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L39) AIR301 [*] `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
- [providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:39:32:](https://github.com/apache/airflow/blob/689608d7423cebf7c72d0e0c0081eceeeac51b3a/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L39) AIR301 `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
+ [providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:39:5:](https://github.com/apache/airflow/blob/689608d7423cebf7c72d0e0c0081eceeeac51b3a/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L39) AIR301 [*] `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
- [providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:39:5:](https://github.com/apache/airflow/blob/689608d7423cebf7c72d0e0c0081eceeeac51b3a/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L39) AIR301 `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
+ [providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:63:17:](https://github.com/apache/airflow/blob/689608d7423cebf7c72d0e0c0081eceeeac51b3a/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L63) AIR301 [*] `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
- [providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:63:17:](https://github.com/apache/airflow/blob/689608d7423cebf7c72d0e0c0081eceeeac51b3a/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L63) AIR301 `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
+ [providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:67:17:](https://github.com/apache/airflow/blob/689608d7423cebf7c72d0e0c0081eceeeac51b3a/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L67) AIR301 [*] `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
- [providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:67:17:](https://github.com/apache/airflow/blob/689608d7423cebf7c72d0e0c0081eceeeac51b3a/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L67) AIR301 `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
this one -> - [providers/common/compat/src/airflow/providers/common/compat/openlineage/utils/utils.py:40:59:](https://github.com/apache/airflow/blob/689608d7423cebf7c72d0e0c0081eceeeac51b3a/providers/common/compat/src/airflow/providers/common/compat/openlineage/utils/utils.py#L40) AIR301 `airflow.providers.openlineage.utils.utils.translate_airflow_dataset` is removed in Airflow 3.0
+ [providers/openlineage/tests/unit/openlineage/plugins/test_utils.py:580:21:](https://github.com/apache/airflow/blob/689608d7423cebf7c72d0e0c0081eceeeac51b3a/providers/openlineage/tests/unit/openlineage/plugins/test_utils.py#L580) AIR301 [*] `airflow.timetables.simple.DatasetTriggeredTimetable` is removed in Airflow 3.0
- [providers/openlineage/tests/unit/openlineage/plugins/test_utils.py:580:21:](https://github.com/apache/airflow/blob/689608d7423cebf7c72d0e0c0081eceeeac51b3a/providers/openlineage/tests/unit/openlineage/plugins/test_utils.py#L580) AIR301 `airflow.timetables.simple.DatasetTriggeredTimetable` is removed in Airflow 3.0
```

I just wanted to make sure that was an expected result before merging.

---

_Comment by @Lee-W on 2025-04-23 00:32_

> - [providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:63:17:](https://github.com/apache/airflow/blob/689608d7423cebf7c72d0e0c0081eceeeac51b3a/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L63) AIR301 `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0

If I'm not mistaken, it seems we'll be running this branch against the Airflow codebase.I think the behavior is correct in this version as the code is guarded by try-catch. I'm not sure why it was there though 🤔 

---

_Comment by @ntBre on 2025-04-23 16:43_

Sounds good, I'll go ahead and merge. Just wanted to make sure that was expected :slightly_smiling_face: 

---

_Merged by @ntBre on 2025-04-23 16:43_

---

_Closed by @ntBre on 2025-04-23 16:43_

---
