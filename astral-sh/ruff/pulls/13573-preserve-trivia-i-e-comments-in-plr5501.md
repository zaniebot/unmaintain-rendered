```yaml
number: 13573
title: Preserve trivia (i.e. comments) in PLR5501
type: pull_request
state: merged
author: zanieb
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: zb/collapse-elif-trivia
created_at: 2024-09-30T16:27:28Z
updated_at: 2024-10-03T15:28:06Z
url: https://github.com/astral-sh/ruff/pull/13573
synced_at: 2026-01-10T20:59:36Z
```

# Preserve trivia (i.e. comments) in PLR5501

---

_Pull request opened by @zanieb on 2024-09-30 16:27_

Closes https://github.com/astral-sh/ruff/issues/13545

As described in the issue, we move comments before the inner `if` statement to before the newly constructed `elif` statement (previously `else`).

---

_Comment by @zanieb on 2024-09-30 19:39_

Needs #13572 before we can get ecosystem checks.

---

_Label `bug` added by @zanieb on 2024-09-30 20:50_

---

_Label `rule` added by @zanieb on 2024-09-30 20:50_

---

_Marked ready for review by @zanieb on 2024-10-01 04:40_

---

_Comment by @github-actions[bot] on 2024-10-01 04:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @MichaReiser by @zanieb on 2024-10-01 18:53_

---

_Review requested from @dhruvmanila by @zanieb on 2024-10-01 18:53_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/collapsible_else_if.rs`:132 on 2024-10-03 06:26_

Is `trivia_range` ever going to be empty? Because there should at least be an indentation between the `else:` line end and the start of the first statement.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/collapsible_else_if.rs`:154 on 2024-10-03 06:28_

Interesting that this works because `safe_edits` accepts an `IntoIterator<item = Edit>` but this is an `Option`. I guess `None` is basically taken as an indicator that the iterator ends and it won't be collected into a vec.

---

_@dhruvmanila approved on 2024-10-03 06:29_

---

_@zanieb reviewed on 2024-10-03 15:00_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/collapsible_else_if.rs`:154 on 2024-10-03 15:00_

Yes `Option` implements `IntoIterator` where it returns an empty iterator when `None`.

---

_@zanieb reviewed on 2024-10-03 15:16_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/collapsible_else_if.rs`:132 on 2024-10-03 15:16_

Good question.. uhh let me look.

---

_@zanieb reviewed on 2024-10-03 15:22_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/collapsible_else_if.rs`:132 on 2024-10-03 15:22_

I put a debug statement in there and the trivia range is indeed empty when there's no trivia 🤷‍♀️ 

---

_Merged by @zanieb on 2024-10-03 15:22_

---

_Closed by @zanieb on 2024-10-03 15:22_

---

_Branch deleted on 2024-10-03 15:22_

---

_@dhruvmanila reviewed on 2024-10-03 15:28_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/collapsible_else_if.rs`:132 on 2024-10-03 15:28_

Yeah, I think it might change when the linter is error resilient but that doesn't need to be addressed now

---
