```yaml
number: 22453
title: "[`airflow`] Second positional argument to Asset/Dataset should not be a dictionary (`AIR303`)"
type: pull_request
state: open
author: sjyangkevin
labels:
  - rule
assignees: []
base: main
head: check-second-pos-arg-in-asset-dataset
created_at: 2026-01-08T04:46:35Z
updated_at: 2026-01-10T13:43:03Z
url: https://github.com/astral-sh/ruff/pull/22453
synced_at: 2026-01-12T15:57:50Z
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
