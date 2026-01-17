```yaml
number: 22453
title: "[`airflow`] Second positional argument to Asset/Dataset should not be a dictionary (`AIR303`)"
type: pull_request
state: open
author: sjyangkevin
labels:
  - rule
  - preview
assignees: []
base: main
head: check-second-pos-arg-in-asset-dataset
created_at: 2026-01-08T04:46:35Z
updated_at: 2026-01-17T17:44:14Z
url: https://github.com/astral-sh/ruff/pull/22453
synced_at: 2026-01-17T18:19:14Z
```

# [`airflow`] Second positional argument to Asset/Dataset should not be a dictionary (`AIR303`)

---

_@sjyangkevin_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR is related to https://github.com/apache/airflow/issues/48389.

In Airflow 3, the function signature for `airflow.Dataset`, `airflow.datasets.Dataset`, and `airflow.sdk.Asset` has changed. Below is the function signature for `airflow.sdk.Asset`. The second positional argument is `uri` which is of type `str`. In the older version, the second positional argument can be a dictionary which is the `extra: 'dict | None' = None` argument. Therefore, we need to flag this in Airflow 3.

```python
Asset(name: 'str | None' = None, uri: 'str | None' = None, *, group: 'str | None' = None, extra: 'dict | None' = None, watchers: 'list[AssetWatcher | SerializedAssetWatcher] | None' = None) -> 'None'
```

As this is a check on constructor call, we need to create a new method `check_constructor_arguments`, instead of on method call. The new rule check whether the index 1 (the second argument) is a dictionary (either a literal or a `dict()` call).

## Test Plan

The `AIR303.py` test file have been updated with new test cases. The snapshot has been updated and all other test cases passed.

@Lee-W , could you please review it when you have a chance, and let me know if you have further feedback. Thanks!

---

_Comment by @astral-sh-bot[bot] on 2026-01-08 04:57_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:150 on 2026-01-08 06:56_

We could mention it's now `Asset(name=..., uri=..., extra=...)`

---

_@Lee-W reviewed on 2026-01-08 06:57_

one nit, LGTM!

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:150 on 2026-01-08 15:34_

wondering if this looks better 

---

_@sjyangkevin reviewed on 2026-01-08 15:34_

---

_Review requested from @Lee-W by @sjyangkevin on 2026-01-08 15:49_

---

_Label `rule` added by @amyreese on 2026-01-08 20:30_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:150 on 2026-01-09 01:16_

```suggestion
                            message: "Use keyword argument `extra` instead of passing a dict as the second positional argument (e.g., `Asset(name=..., uri=..., extra=...)`)",
```

---

_@Lee-W reviewed on 2026-01-09 01:16_

LGTM after the last nit 👍 Thanks!

---

_@sjyangkevin reviewed on 2026-01-09 01:19_

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:150 on 2026-01-09 01:19_

cool! just pushed the fix.

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:1 on 2026-01-09 15:38_

wonder if we should update the version as 0.14.11 was released.
```
#[violation_metadata(preview_since = "0.14.11")]
```


---

_@sjyangkevin reviewed on 2026-01-09 15:39_

---

_Review requested from @ntBre by @MichaReiser on 2026-01-09 16:36_

---

_@Lee-W reviewed on 2026-01-10 13:43_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:1 on 2026-01-10 13:43_

yep, I think so

---

_@sjyangkevin reviewed on 2026-01-15 15:09_

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:1 on 2026-01-15 15:09_

actually, I just checked again on this. As AIR303 is included in the release "0.14.11". If I understand correctly, the preview version should probably still be "0.14.11" as we first introduce the code/rules in that version. This PR adds one more rule on top of it.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:1 on 2026-01-15 15:27_

This should be fine as 0.14.11. This field is for when the rule is first released, so you don't need to update it when modifying an existing rule (until we stabilize the rule) 👍 

---

_Label `preview` added by @ntBre on 2026-01-15 15:27_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:161 on 2026-01-15 15:35_

There are a couple of other ways you could check this, if you want. The simplest is to check for `Expr::DictComp` (dict comprehension) as well as a dict literal. And then a more invasive check would be [`typing::is_dict`](https://github.com/astral-sh/ruff/blob/652783790aea7d8ad7dbd22b6960876f7c699928/crates/ruff_python_semantic/src/analyze/typing.rs#L1035), which will check if a variable binding can be resolved to a `dict` based on type annotations.

It's okay with me if you want to restrict the check to dict literals and `dict()` calls, though. We should just add tests for these other cases if you add additional checks.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:143 on 2026-01-15 15:37_

Is this argument positional-only? I think it could also be passed as a keyword based on the signature in the PR description. We could use `find_argument_value` if you want to handle the keyword case too.

---

_@ntBre reviewed on 2026-01-15 15:39_

Thank you! This looks good to me overall, just a couple of minor suggestions about additional cases you could handle, if you want.

---

_@sjyangkevin reviewed on 2026-01-15 16:57_

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:143 on 2026-01-15 16:57_

Thanks for the feedback!! If I understand the question correctly, and I think yes, the second argument `uri` can be passed as keyword based on the current function signature. I think the second positional argument in the old version is used to set the `extra`, but now it is changed to set the `uri`. This check then flags this change. Hope this answer the question.

```python
Dataset("ds1", {"some_extra": 1})
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:143 on 2026-01-15 20:04_

Ohhh okay, I think I understand. So you're not worried about a case like:

```py
Dataset("ds1", uri="...")
```

because that's still valid in the new API? The problem is specifically when the second positional argument is a dict. I think this is fine as-is then. Thanks!

---

_@ntBre reviewed on 2026-01-15 20:04_

---

_@sjyangkevin reviewed on 2026-01-15 22:47_

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:161 on 2026-01-15 22:47_

Thanks for pointing this out. I searched through the codebase and find a similar example to perform such test. I've made the update to the check logic, as well as adding test cases for other variants.

---

_Review requested from @ntBre by @sjyangkevin on 2026-01-15 23:07_

---

_Review requested from @Lee-W by @sjyangkevin on 2026-01-15 23:07_

---

_@ntBre approved on 2026-01-16 21:18_

Looks good to me, if @Lee-W is happy with it too! Thank you!

---

_@Lee-W reviewed on 2026-01-17 07:02_

---

_Review comment by @Lee-W on `crates/ruff_linter/resources/test/fixtures/airflow/AIR303.py`:75 on 2026-01-17 07:02_

```suggestion
```

nit: additional blank line

---

_@Lee-W reviewed on 2026-01-17 07:02_

Just took a look again. LGTM. Thanks all!

---

_@sjyangkevin reviewed on 2026-01-17 17:34_

---

_Review comment by @sjyangkevin on `crates/ruff_linter/resources/test/fixtures/airflow/AIR303.py`:75 on 2026-01-17 17:34_

fixed. thanks for caching it.

---
