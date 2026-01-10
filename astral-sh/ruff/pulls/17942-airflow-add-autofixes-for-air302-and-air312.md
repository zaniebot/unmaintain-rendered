```yaml
number: 17942
title: "[`airflow`] Add autofixes for `AIR302` and `AIR312`"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: refactor-AIR302-ARI312
created_at: 2025-05-08T10:12:22Z
updated_at: 2025-05-15T20:03:03Z
url: https://github.com/astral-sh/ruff/pull/17942
synced_at: 2026-01-10T18:51:01Z
```

# [`airflow`] Add autofixes for `AIR302` and `AIR312`

---

_Pull request opened by @Lee-W on 2025-05-08 10:12_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

`ProviderReplacement::Name` was designed back when we only wanted to do linting. Now we also want to fix the user code. It would be easier for us to replace them with better AutoImport struct.

## Test Plan

<!-- How was it tested? -->

The test fixture has been updated as some cases can now be fixed


---

_Comment by @github-actions[bot] on 2025-05-08 10:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "refactor(AIR302,-AIR312): get rid of ProviderReplacement::ProviderNam…" to "[`airflow`] Get rid of `ProviderName::Name` and replace them with `ProviderName::AutoImport` for enabling auto fixing" by @Lee-W on 2025-05-08 11:52_

---

_Renamed from "[`airflow`] Get rid of `ProviderName::Name` and replace them with `ProviderName::AutoImport` for enabling auto fixing" to "[`airflow`] Get rid of `ProviderName::Name` and replace them with `ProviderName::AutoImport` for enabling auto fixing (`AIR302`, `AIR312`)" by @Lee-W on 2025-05-08 11:52_

---

_Marked ready for review by @Lee-W on 2025-05-08 11:53_

---

_Renamed from "[`airflow`] Get rid of `ProviderName::Name` and replace them with `ProviderName::AutoImport` for enabling auto fixing (`AIR302`, `AIR312`)" to "[`airflow`] Get rid of `ProviderReplacement::Name` and replace them with `ProviderReplacement::AutoImport` for enabling auto fixing (`AIR302`, `AIR312`)" by @Lee-W on 2025-05-08 11:59_

---

_Review requested from @ntBre by @ntBre on 2025-05-13 01:39_

---

_@ntBre approved on 2025-05-14 15:15_

Makes sense, thanks!

---

_Comment by @ntBre on 2025-05-14 15:15_

Just need to fix the very minor clippy lint.

---

_Comment by @Lee-W on 2025-05-15 02:53_

Thanks for reminding me! I forgot to backport the fix to this one.

---

_Label `rule` added by @ntBre on 2025-05-15 20:02_

---

_Label `preview` added by @ntBre on 2025-05-15 20:02_

---

_Renamed from "[`airflow`] Get rid of `ProviderReplacement::Name` and replace them with `ProviderReplacement::AutoImport` for enabling auto fixing (`AIR302`, `AIR312`)" to "[`airflow`] Add autofixes for `AIR302` and `AIR312`" by @ntBre on 2025-05-15 20:02_

---

_Merged by @ntBre on 2025-05-15 20:03_

---

_Closed by @ntBre on 2025-05-15 20:03_

---
