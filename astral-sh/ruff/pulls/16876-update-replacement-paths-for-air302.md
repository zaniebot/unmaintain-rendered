```yaml
number: 16876
title: Update replacement paths for AIR302
type: pull_request
state: merged
author: ashb
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-airflow302-replacment-paths
created_at: 2025-03-20T17:17:49Z
updated_at: 2025-03-24T12:28:16Z
url: https://github.com/astral-sh/ruff/pull/16876
synced_at: 2026-01-10T19:40:36Z
```

# Update replacement paths for AIR302

---

_Pull request opened by @ashb on 2025-03-20 17:17_

I am one of the core developers of Airflow and working on the "airflow.sdk"
package, and this updates the recommended replacments to the correct
user-facing imports.[^1]

cc @Lee-W @uranusjr 

[^1]: https://github.com/apache/airflow/blob/33f0f1d639b53d4221757c8493b819684bd26d21/task-sdk/src/airflow/sdk/__init__.py#L68-L93

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Hope and pray? 😉 

I'm sure there are some snapshot files I'm supposed to fix first.


<!-- How was it tested? -->


---

_Label `rule` added by @dhruvmanila on 2025-03-21 04:05_

---

_Label `preview` added by @dhruvmanila on 2025-03-21 04:05_

---

_Comment by @dhruvmanila on 2025-03-21 04:07_

> I'm sure there are some snapshot files I'm supposed to fix first.

Yeah, you can run the following command to test:
```
cargo insta test -p ruff_linter --unreferenced reject 
```
And, the following to verify the snapshots:
```
cargo insta review
```

---

_Comment by @github-actions[bot] on 2025-03-21 09:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@Lee-W reviewed on 2025-03-21 09:47_

LGTM 🎉 

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:618 on 2025-03-21 09:49_

We can combine these two to avoid the `Replacement` duplication:

```rs
["airflow", "Dataset"] | ["airflow", "datasets", "Dataset"] => ...
```

---

_@dhruvmanila approved on 2025-03-21 09:49_

---

_@ashb reviewed on 2025-03-21 15:47_

---

_Review comment by @ashb on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:618 on 2025-03-21 15:47_

Done

---

_Merged by @MichaReiser on 2025-03-21 17:46_

---

_Closed by @MichaReiser on 2025-03-21 17:46_

---

_Branch deleted on 2025-03-24 12:28_

---
