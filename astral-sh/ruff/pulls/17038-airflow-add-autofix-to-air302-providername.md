```yaml
number: 17038
title: "[airflow] add autofix to AIR302 ProviderName"
type: pull_request
state: closed
author: Lee-W
labels:
  - fixes
  - preview
assignees: []
draft: true
base: main
head: autofix-AIR303
created_at: 2025-03-28T15:11:33Z
updated_at: 2025-04-23T02:45:37Z
url: https://github.com/astral-sh/ruff/pull/17038
synced_at: 2026-01-10T19:33:02Z
```

# [airflow] add autofix to AIR302 ProviderName

---

_Pull request opened by @Lee-W on 2025-03-28 15:11_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Add autofix to AIR303 name moved to provider

## Test Plan

<!-- How was it tested? -->
The existing test fixture reflects the update

---

_Comment by @Lee-W on 2025-03-28 15:11_

this branch based on https://github.com/astral-sh/ruff/pull/16965

---

_Comment by @github-actions[bot] on 2025-03-28 15:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dhruvmanila on 2025-03-31 20:53_

(I'll avoid reviewing this PR for now as it's in draft and it's based on a different branch; the diff includes code from both branches. Once the dependent PR is merged, we can move this out of draft and I can review it.)

---

_Label `fixes` added by @dhruvmanila on 2025-03-31 21:04_

---

_Label `preview` added by @dhruvmanila on 2025-03-31 21:04_

---

_Marked ready for review by @Lee-W on 2025-04-02 15:37_

---

_Comment by @Lee-W on 2025-04-02 15:48_

@dhruvmanila This PR is ready now 🙌  (after all the merging and rebasing 💦)

---

_Converted to draft by @Lee-W on 2025-04-02 16:07_

---

_Comment by @dhruvmanila on 2025-04-02 17:17_

> @dhruvmanila This PR is ready now 🙌 (after all the merging and rebasing 💦)

You've put this back into draft, is that intentional?

---

_Comment by @Lee-W on 2025-04-03 02:17_

Yep, it is. Found an issue after a few more rebasing. Was trying to fix it but couldn't do it on time. 🥲

---

_Renamed from "[airflow] add autofix to AIR303 ProviderName" to "[airflow] add autofix to AIR302 ProviderName" by @Lee-W on 2025-04-07 15:20_

---

_@Lee-W reviewed on 2025-04-07 15:24_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:148 on 2025-04-07 15:24_

Hey @ntBre, a quick question. I tried to apply autofix to all the similar symbol changes, but only a few of them can be fixed. Do you know what might be missing?

---

_@ntBre reviewed on 2025-04-07 16:17_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:148 on 2025-04-07 16:17_

I might be missing some context here. What kind of autofix were you trying to apply? Or what change were you trying to make?

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:148 on 2025-04-08 13:10_

@ntBre

for example, if the user is using

```python
from airflow.operators.google_api_to_s3_transfer import GoogleApiToS3Operator

GoogleApiToS3Operator()
```

we want to fix it as 

```python
from airflow.providers.amazon.aws.transfers.google_api_to_s3 import GoogleApiToS3Operator

GoogleApiToS3Operator()
```

---

_@Lee-W reviewed on 2025-04-08 13:10_

---

_@ntBre reviewed on 2025-04-08 18:27_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:148 on 2025-04-08 18:27_

I tried adding an `unwrap` call instead of `?` on the `get_or_import_symbol` call, and I'm seeing this error when running the AIR302 tests:

```
called `Result::unwrap()` on an `Err` value: ConflictingName("provide_bucket_name")
```

It looks like you can't overwrite an existing symbol with a different import path, at least through the `get_or_import_symbol` API.

I don't really have a great suggestion for working around this. You could try a direct text replacement, but I can easily imagine that going wrong if there are multiple imports grouped together or something like that.

Maybe a better alternative would be a custom version of [`Importer::import_symbol`](https://github.com/astral-sh/ruff/blob/7ff8b6b293e3c8812d6bc5cd4416fa585f13196f/crates/ruff_linter/src/importer/mod.rs#L377) (inside of a custom `get_or_import_symbol`) that ignores the `ConflictingName` error case since you're overwriting it anyway.

---

_@dhruvmanila reviewed on 2025-04-08 19:05_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:148 on 2025-04-08 19:05_

Ah yes, I think that updating an existing import isn't currently supported and you'd need to add a dedicated edit infrastructure for it. The NumPy deprecation rules doesn't support that either so it should be fine for now unless you'd want to add support for it.

---

_@dhruvmanila reviewed on 2025-04-08 19:07_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:148 on 2025-04-08 19:07_

Although this doesn't look too difficult and we could potentially use a text replacement as suggested by Brent.

---

_@Lee-W reviewed on 2025-04-09 10:27_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:148 on 2025-04-09 10:27_

I think that's crucial for us. I'll explore a bit and see how I can achieve it. Before that, I'll apply `AutoImport` to fixable ones.

---

_Assigned to @ntBre by @ntBre on 2025-04-09 14:37_

---

_Closed by @Lee-W on 2025-04-23 02:45_

---
