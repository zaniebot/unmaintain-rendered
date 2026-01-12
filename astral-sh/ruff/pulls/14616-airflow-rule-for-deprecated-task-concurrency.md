```yaml
number: 14616
title: "[airflow] rule for deprecated task_concurrency parameter (AIR303)"
type: pull_request
state: closed
author: abhishekbhakat
labels:
  - rule
  - preview
assignees: []
draft: true
base: main
head: deprecated-task-concurrency
created_at: 2024-11-26T17:48:57Z
updated_at: 2025-05-05T15:38:27Z
url: https://github.com/astral-sh/ruff/pull/14616
synced_at: 2026-01-12T15:55:48Z
```

# [airflow] rule for deprecated task_concurrency parameter (AIR303)

---

_@abhishekbhakat_

## Summary
Airflow 3.0 renames the `task_concurrency` parameter to `max_active_tis_per_dag` across all operators. The old parameter will no longer have any effect in 3.0. This rule detects usage of the deprecated parameter to help users migrate their DAGs to the new concurrency control pattern.

Ref: #14626

## Test Plan
A test fixture has been included for the rule.

---

_Comment by @MichaReiser on 2024-11-26 17:52_

So many airflow rules :) 

Are you working/coordinating with @uranusjr ? I just want to ensure we have "one" authority that has a big picture in mind for all tne new airflow rules.

---

_Label `rule` added by @MichaReiser on 2024-11-26 17:52_

---

_Label `preview` added by @MichaReiser on 2024-11-26 17:52_

---

_Comment by @github-actions[bot] on 2024-11-26 17:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @abhishekbhakat on 2024-11-26 17:58_

Yes and a whole lot more coming.

---

_Closed by @abhishekbhakat on 2024-11-26 18:44_

---

_Reopened by @abhishekbhakat on 2024-11-26 18:50_

---

_Converted to draft by @abhishekbhakat on 2024-11-26 18:50_

---

_Renamed from "rule for deprecated task_concurrency parameter" to "[airflow] rule for deprecated task_concurrency parameter (AIR303)" by @abhishekbhakat on 2024-11-26 18:50_

---

_Closed by @abhishekbhakat on 2024-11-26 19:10_

---

_Reopened by @abhishekbhakat on 2024-11-27 05:31_

---

_Marked ready for review by @abhishekbhakat on 2024-11-27 05:32_

---

_@dhruvmanila reviewed on 2024-11-27 06:03_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/task_concurrency.rs`:63 on 2024-11-27 06:03_

As https://github.com/astral-sh/ruff/pull/14581 is merged we should update this to use the `seen_module` at the top of the rule function:

```rs
    if !checker.semantic().seen_module(Modules::AIRFLOW) {
        return;
    }
```

The semantic model caches common module lookups and exposes a method to allow us to query it.

---

_Comment by @dhruvmanila on 2024-11-27 06:13_

Are there other parameters that have been deprecated? I wonder if we should club them under a single rule similar to the NumPy 2.0 deprecations where one rule would look at the imports and the other would look at function parameters.

---

_Comment by @abhishekbhakat on 2024-11-27 06:19_

There are a lot of breaking changes in Airflow 3 coming from Airflow 2.
Here is an exhaustive list https://github.com/apache/airflow/issues/41641 
We are picking one by one. Will try to club them wherever possible.

---

_@abhishekbhakat reviewed on 2024-11-27 06:50_

---

_Review comment by @abhishekbhakat on `crates/ruff_linter/src/rules/airflow/rules/task_concurrency.rs`:63 on 2024-11-27 06:50_

Done

---

_Comment by @MichaReiser on 2024-11-27 08:16_

Oh wow, that's a very long list. I didn't expect there to be that many changes. 

I think it would be great to write up a short proposal on what rules you plan on adding and how you want to group the deprecations. We want to avoid having 50ish airflow deprecation rules. For example: Can we have one rule for:

* removals (with a possible fix suggesting the new name)
* deprecations
* new defaults (or similar)

We can incrementally add to those rules. 

I think a good place to discuss the new rules is https://github.com/astral-sh/ruff/issues/14626

---

_Review comment by @kaxil on `crates/ruff_linter/resources/test/fixtures/airflow/AIR303.py`:7 on 2024-11-27 11:35_

@abhishekbhakat We should raise a deprecation error in Airflow 2.2+ too: https://github.com/apache/airflow/pull/17708 (that was when we had original deprecation)

In Airflow 3, it is "removed" entirely. 

---

_@kaxil reviewed on 2024-11-27 11:35_

---

_Comment by @MichaReiser on 2024-12-09 16:00_

What's the status of this rule? Is it still needed according to the new categorization?

---

_Comment by @abhishekbhakat on 2024-12-09 16:25_

I'll put this as a draft PR. We are working on the consolidated rules. We'll revisit this after that is ready.

---

_Converted to draft by @abhishekbhakat on 2024-12-09 16:25_

---

_Comment by @uranusjr on 2025-05-05 10:20_

Do we still need this?

---

_Comment by @abhishekbhakat on 2025-05-05 15:38_

Didn't kept a track of what has been merged yet. But then again we can revisit it in future if needed. Closing this PR for now.

---

_Closed by @abhishekbhakat on 2025-05-05 15:38_

---
