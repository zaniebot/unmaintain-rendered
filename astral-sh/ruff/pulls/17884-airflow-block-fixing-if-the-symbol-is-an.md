```yaml
number: 17884
title: "[`airflow`] Block fixing if the symbol is an attribute access (`AIR3`)"
type: pull_request
state: closed
author: Lee-W
labels:
  - rule
  - preview
assignees: []
draft: true
base: main
head: block-attr-fix
created_at: 2025-05-06T08:25:27Z
updated_at: 2025-06-21T06:27:07Z
url: https://github.com/astral-sh/ruff/pull/17884
synced_at: 2026-01-12T15:56:07Z
```

# [`airflow`] Block fixing if the symbol is an attribute access (`AIR3`)

---

_@Lee-W_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixing cases like 

```python
from airflow import datasets
datasets.Dataset()
```

can be confusing in many cases. As Airflow 3 changes some path dramatically, the numbers of module parts might not even be equal

## Test Plan

<!-- How was it tested? -->
test fixtures are added for this


---

_Comment by @github-actions[bot] on 2025-05-06 08:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "Block attr fix" to "[`airflow`] Block fixing if the symbol is an attribute access (`AIR3`)" by @Lee-W on 2025-05-06 09:37_

---

_Marked ready for review by @Lee-W on 2025-05-06 09:40_

---

_Label `rule` added by @ntBre on 2025-05-06 20:24_

---

_Label `preview` added by @ntBre on 2025-05-06 20:24_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:939 on 2025-05-06 20:31_

nit: I think there should be a helper like `expr.is_name_expr` (or `expr_name`?) to use instead of if-let since we're not using the let binding.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:880 on 2025-05-06 20:33_

Same as above.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_move_to_provider_in_3.rs`:282 on 2025-05-06 20:33_

Same as above.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_update_3_0.rs`:290 on 2025-05-06 20:33_

Same as above.

---

_@ntBre approved on 2025-05-06 20:41_

Thanks, the code changes look good to me. However, I might be slightly confused about the intention here. Do you think adding the additional imports will be too confusing for users? It seems like it would still be more convenient to get the fix and then format your imports afterward, or something like that, but I could be wrong.

---

_Comment by @Lee-W on 2025-05-07 00:34_

> Thanks, the code changes look good to me. However, I might be slightly confused about the intention here. Do you think adding the additional imports will be too confusing for users? It seems like it would still be more convenient to get the fix and then format your imports afterward, or something like that, but I could be wrong.

The main reason is it could be wrongly fixed. 

The example in the description will be fixed as 

```python
from __future__ import annotations

from airflow import datasets
from airflow.sdk import Asset

datasets.Asset()
```

which is wrong

---

_Comment by @MichaReiser on 2025-05-07 06:43_

@Lee-W can you explain what's wrong about it? Is it that it unnecessarily imports `Asset` or is the `datasets.Asset` accessor wrong?

---

_Comment by @Lee-W on 2025-05-07 07:44_

> @Lee-W can you explain what's wrong about it? Is it that it unnecessarily imports `Asset` or is the `datasets.Asset` accessor wrong?

`airflow.datasets.Dataset` is updated as `airflow.sdk.Asset` in Airflow 3.0

`datasets.Asset` is an error, and `Asset` is unnecessarily imported

---

_@Lee-W reviewed on 2025-05-07 08:53_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:939 on 2025-05-07 08:53_

Just updated it. Thanks!

---

_@Lee-W reviewed on 2025-05-07 08:53_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:880 on 2025-05-07 08:53_

Just updated it. Thanks!

---

_@Lee-W reviewed on 2025-05-07 08:53_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_move_to_provider_in_3.rs`:282 on 2025-05-07 08:53_

Just updated it. Thanks!

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_update_3_0.rs`:290 on 2025-05-07 08:53_

Just updated it. Thanks!

---

_@Lee-W reviewed on 2025-05-07 08:53_

---

_Comment by @ntBre on 2025-05-07 18:19_

I could still be misunderstanding something, but it sounds like the right fix in this case would be to replace the whole `datasets.Dataset` with `Asset`? I don't remember the details of how the replacements are generated, so maybe there's a reason that would be difficult.

---

_Comment by @Lee-W on 2025-05-08 01:16_

Ideally (even this ideal case might not be 100% correct)

```python
from airflow import datasets

datasets.Datasets()
```

should be replaced as 

```python
from airflow import sdk

sdk.Asset()
```

---

However, this could introduce issues if we have something like 

```python
from airflow import io

io.path.ObjectStoragePath()


# `airflow.io.path.ObjectStoragePath` is updated as `airflow.sdk.ObjectStoragePath` in Airflow 3
#
# Should we just replace `io` with `sdk`?
# But some other names might still use `io`.
#
# Or should it be something like
# 
# from airflow import sdk
# sdk.ObjectStoragePath()
#
# but the count doesn't match, how should we achieve this? even if we achieve this, is it even correct?
```

These cases really depend on how the users use it and it hard to define what is the right fix.

---

_Comment by @ntBre on 2025-05-08 14:26_

I think if you don't worry about trying to remove existing imports, everything should be okay right? In this case:

```py
from airflow import datasets

datasets.Datasets()
```

I'd expect this transformation:

```py
from airflow import datasets  # original import untouched
from airflow import sdk  # new import

sdk.Asset()
```

I think you could get the new `sdk` import by requesting  that from the `Importer` rather than the full path and then just replace `datasets.Datasets` with `sdk.Asset`. The importer should handle raising an error and avoiding a fix if there's a conflicting import.

And I think another rule ([unused-import (F401)](https://docs.astral.sh/ruff/rules/unused-import/#unused-import-f401)) should clean up any imports that are unused at the end of the transformation, if it's also enabled.

I'm happy deferring to you here, but I was hopeful that we'd have an existing API that could handle what you needed!

---

_Comment by @Lee-W on 2025-05-08 15:46_

> I think if you don't worry about trying to remove existing imports, everything should be okay right? In this case:
> 
> ```python
> from airflow import datasets
> 
> datasets.Datasets()
> ```
> 
> I'd expect this transformation:
> 
> ```python
> from airflow import datasets  # original import untouched
> from airflow import sdk  # new import
> 
> sdk.Asset()
> ```
> 
> I think you could get the new `sdk` import by requesting that from the `Importer` rather than the full path and then just replace `datasets.Datasets` with `sdk.Asset`. The importer should handle raising an error and avoiding a fix if there's a conflicting import.
> 
> And I think another rule ([unused-import (F401)](https://docs.astral.sh/ruff/rules/unused-import/#unused-import-f401)) should clean up any imports that are unused at the end of the transformation, if it's also enabled.
> 
> I'm happy deferring to you here, but I was hopeful that we'd have an existing API that could handle what you needed!

The current behavior still cannot replace `datasets.Dataset` with `sdk.Asset`. It replaces `datasets.Dataset` with `datasets.Asset`, which breaks users' code.

This kind of fixing could be problematic, as the import path length can differ.
In the following example, it's hard to define how to fix it and why it should be fixed that way.

```python
from airflow import io

io.path.ObjectStoragePath()

# airflow.sdk.ObjectStoragePath in Airflow 3
```

Thus, I think we should avoid this kind of attribute fixing as the risk could be high

---

_Comment by @MichaReiser on 2025-05-09 15:52_

Could we use some heuristic to determine what pattern the user is using and provide a fix in those cases (assuming there are some prominent usages?). It seems a shame to remove the fix.

---

_Converted to draft by @Lee-W on 2025-05-10 00:26_

---

_Comment by @Lee-W on 2025-05-10 00:30_

> Could we use some heuristic to determine what pattern the user is using and provide a fix in those cases (assuming there are some prominent usages?). It seems a shame to remove the fix.

I think the most prominent case is something like

```python
from airflow.datasets import Dataset
Dataset()
```

which is correctly fixed already. What I'm doing in this PR is to prevent fixes that are known to be incorrect. As for the correct solution, it really depends on various factors, and I don't have a clear answer at this time. I plan to revisit this issue later. Thus, mark this PR as a draft.

---

_Comment by @MichaReiser on 2025-06-20 06:42_

What's the status on this PR? 

---

_Comment by @Lee-W on 2025-06-21 06:27_

We've not yet had a chance to go back to this one. let me close it and create a new one till we have some bandwidth to work on it

---

_Closed by @Lee-W on 2025-06-21 06:27_

---
