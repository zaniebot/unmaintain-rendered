```yaml
number: 17115
title: "[`airflow`] Add missing `AIR302` attribute check"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-more-AIR303-condition
created_at: 2025-04-01T11:21:35Z
updated_at: 2025-04-09T17:58:42Z
url: https://github.com/astral-sh/ruff/pull/17115
synced_at: 2026-01-10T19:40:37Z
```

# [`airflow`] Add missing `AIR302` attribute check

---

_Pull request opened by @Lee-W on 2025-04-01 11:21_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

attribute check was missing in the previous implementation

e.g.

```python
from airflow.api.auth.backend import basic_auth

basic_auth.auth_current_user
```

This PR adds this kind of check.

## Test Plan

<!-- How was it tested? -->

The test case has been added to the button of the existing test fixtures, confirmed to be correct and later reorgnaized


---

_Comment by @github-actions[bot] on 2025-04-01 11:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sunank200 reviewed on 2025-04-02 06:27_

LGTM

---

_Label `rule` added by @dhruvmanila on 2025-04-02 15:09_

---

_Label `preview` added by @dhruvmanila on 2025-04-02 15:09_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:892 on 2025-04-02 15:12_

This seems to be creating two diagnostics that are same for the two parts of the attribute node. Can you look into it?

---

_@dhruvmanila requested changes on 2025-04-02 15:12_

---

_@Lee-W reviewed on 2025-04-02 15:17_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:892 on 2025-04-02 15:17_

I think it's expected for `"airflow", "api", "auth", "backend", "basic_auth", ..`. `...basic_auth` or anything under `basic_auth`

---

_@Lee-W reviewed on 2025-04-02 15:18_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:892 on 2025-04-02 15:18_

to reduce this kind of confusion. We're also going to remove `Replacement::ImportPathMoved` and expand them to individual `Replacement::ProviderName` instead.

But I think the logic in this PR is still required

---

_@dhruvmanila reviewed on 2025-04-02 15:25_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:892 on 2025-04-02 15:25_

I'm not sure I follow here. There are two duplicate diagnostics for separate symbol:
* `basic_auth`
* `auth_current_user`

They both say:
```
Import path `airflow.api.auth.backend.basic_auth` is moved into `fab` provider in Airflow 3.0;
```

But, we should avoid the duplicate diagnostic and only highlight it once.

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:892 on 2025-04-02 15:32_

I think `basic_auth` was check when it is passed into as a Name and `auth_current_user` is checked when it's an attribute under `basic_auth` (because of `["airflow", "api", "auth", "backend", "basic_auth", ..]`)

The issue occurs because we implemented the path check only instead of checking the exact name. This is antoher issue (which will be addressed when we replace all the path check with exact name check)

---

This PR intends to solve the following issue. (I'll update the test fixture)

```python
from airflow.operators import python

python.PythonOperator()
```


---

_@Lee-W reviewed on 2025-04-02 15:32_

---

_@dhruvmanila reviewed on 2025-04-02 15:59_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:892 on 2025-04-02 15:59_

I understand that part, but I think the bug related to duplicate diagnostic would still exists and we should address that. I'm not exactly sure how as I'd need to look at the code first.

---

_@Lee-W reviewed on 2025-04-02 16:05_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:892 on 2025-04-02 16:05_

I just updated the test fixture to the issue we have. 

Yep, we'll address that issue as well. but that would take sometime to replace `Replacement::ImportPath` as `Replacement::Provider`. @sunank200 and I already sync up and we're thinking of making them another PR. WDYT?

---

_@Lee-W reviewed on 2025-04-02 16:05_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:892 on 2025-04-02 16:05_

It's also ok if we want it to be in this PR 🙂 

---

_@dhruvmanila reviewed on 2025-04-02 17:18_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:892 on 2025-04-02 17:18_

We want to do a release this week and I'm worried that this might not be fixed until then. I'd prefer that we do it in this PR itself.

---

_@Lee-W reviewed on 2025-04-03 02:18_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:892 on 2025-04-03 02:18_

Got it. Sounds good

---

_Converted to draft by @Lee-W on 2025-04-03 02:22_

---

_Renamed from "[airflow] Add misssing AIR303 attribute check" to "[airflow] Add misssing AIR302 attribute check" by @Lee-W on 2025-04-03 03:05_

---

_@Lee-W reviewed on 2025-04-08 03:32_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:892 on 2025-04-08 03:32_

It's fixed in https://github.com/astral-sh/ruff/pull/17278

---

_Comment by @Lee-W on 2025-04-08 03:33_

This PR is now based on https://github.com/astral-sh/ruff/pull/17278

---

_Marked ready for review by @Lee-W on 2025-04-08 13:11_

---

_Comment by @Lee-W on 2025-04-08 13:12_

Thanks @ntBre, for merging #17278! This one is ready as well

---

_Comment by @Lee-W on 2025-04-08 13:13_

I guess we might need @dhruvmanila to confirm as well?

---

_Renamed from "[airflow] Add misssing AIR302 attribute check" to "[airflow] Add missing AIR302 attribute check" by @Lee-W on 2025-04-08 13:20_

---

_Comment by @ntBre on 2025-04-09 14:48_

I can override Dhruv's review if needed :wink: Can you fix the merge conflicts? Then I'll try to review today.

---

_Comment by @Lee-W on 2025-04-09 15:10_

Just updated! 🙏

---

_Assigned to @ntBre by @ntBre on 2025-04-09 17:24_

---

_@ntBre approved on 2025-04-09 17:44_

This looks good to me!

---

_Renamed from "[airflow] Add missing AIR302 attribute check" to "[`airflow`] Add missing `AIR302` attribute check" by @ntBre on 2025-04-09 17:45_

---

_Merged by @ntBre on 2025-04-09 17:58_

---

_Closed by @ntBre on 2025-04-09 17:58_

---
