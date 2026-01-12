```yaml
number: 15240
title: "feat(AIR302): check whether removed context key \"conf\" is access through context.get(...)"
type: pull_request
state: closed
author: Lee-W
labels:
  - rule
  - preview
assignees: []
base: main
head: context-get-invalid-key
created_at: 2025-01-03T15:32:51Z
updated_at: 2025-01-23T08:50:13Z
url: https://github.com/astral-sh/ruff/pull/15240
synced_at: 2026-01-12T15:55:50Z
```

# feat(AIR302): check whether removed context key "conf" is access through context.get(...)

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

context key `conf` is removed in Airflow3. This PR handle the case that these keys are accessed through `context.get(...)`

e.g.,

```python
@task
def func(**context):
    context.get("conf")

@task()
def func(**context):
    context.get("conf")
```

## Test Plan

<!-- How was it tested? -->
A test fixture is included in the PR.


---

_Comment by @Lee-W on 2025-01-03 15:35_

This PR also adds a utility function and works as an example to check whether this kind of access is from taskflow. for https://github.com/astral-sh/ruff/pull/15144

---

_Comment by @Lee-W on 2025-01-03 15:37_

@sunank200 As I'll be out traveling for the following week and will have almost no access to laptop, if there're comments to be resolved, please feel free to push to this branch directly and this PR should help https://github.com/astral-sh/ruff/pull/15144 moving forward as well.

---

_Comment by @github-actions[bot] on 2025-01-03 15:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:228 on 2025-01-03 15:39_

```suggestion
    // Ensure the method called is on `context`
```

---

_Comment by @Lee-W on 2025-01-03 15:40_

I'm not able to finish the python_callable part in time but that part should be checked as well as discussed in https://github.com/astral-sh/ruff/pull/15144#discussion_r1900608421

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:218 on 2025-01-03 15:41_

How many arguments are you planning to deprecate for this to be a list? 

---

_@MichaReiser reviewed on 2025-01-03 15:41_

---

_Comment by @MichaReiser on 2025-01-03 15:42_

> As I'll be out traveling for the following week and will have almost no access to laptop, 

Safe and good travels! Enjoy your time without a laptop ;)

---

_@dhruvmanila reviewed on 2025-01-06 10:35_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:914 on 2025-01-06 10:35_

We can possible make this generic as I suspect it's going to be used for other decorators as well. Ignore if it's not going to be used for any other decorators other than `@task`.

```rs
fn is_decorated_with(checker: &mut Checker, decorator: &str) -> bool {
    let mut parents = checker.semantic().current_statements();
    if let Some(Stmt::FunctionDef(StmtFunctionDef { decorator_list, .. })) =
        parents.find(|stmt| stmt.is_function_def_stmt())
    {
        for decorator in decorator_list {
            if checker
                .semantic()
                .resolve_qualified_name(map_callable(&decorator.expression))
                .is_some_and(|qualified_name| {
                    matches!(qualified_name.segments(), ["airflow", "decorators", decorator])
                })
            {
                return true;
            }
        }
    }
    false
}

---

_Label `rule` added by @dhruvmanila on 2025-01-06 10:36_

---

_Label `preview` added by @dhruvmanila on 2025-01-06 10:36_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:914 on 2025-01-06 12:58_

@task is probably the only case I can think of for now 🤔　I can do the refactoring once we have another case in the future 

---

_@Lee-W reviewed on 2025-01-06 12:58_

---

_Comment by @sunank200 on 2025-01-06 14:20_

@Lee-W I have made the generic change here: https://github.com/astral-sh/ruff/pull/15144

---

_@dhruvmanila reviewed on 2025-01-07 11:37_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:218 on 2025-01-07 11:37_

I think there are multiple as stated in this PR description: https://github.com/astral-sh/ruff/pull/15144#issue-2759448217

---

_Comment by @dhruvmanila on 2025-01-07 11:38_

@sunank200 This PR and the one that you've opened (#15144) seems very familiar. Does your PR supersedes this? If not, can / should they be merged?

---

_Comment by @sunank200 on 2025-01-08 06:14_

@dhruvmanila this PR [#15144](https://github.com/astral-sh/ruff/pull/15144) supersedes this. We could keep [#15144](https://github.com/astral-sh/ruff/pull/15144). I can make @Lee-W coauthor on my PR.

---

_Comment by @dhruvmanila on 2025-01-08 10:05_

Sounds good @sunank200, thanks. I'll close this PR then.

---

_Closed by @dhruvmanila on 2025-01-08 10:05_

---

_Comment by @Lee-W on 2025-01-13 23:30_

Thanks all for helping out! Yep, the intention of this PR was just to help #15144.

---

_Branch deleted on 2025-01-23 08:50_

---
