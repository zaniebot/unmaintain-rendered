```yaml
number: 21096
title: "[`airflow`] passing positional argument into `airflow.lineage.hook.HookLineageCollector.create_asset` is not allowed (`AIR301`)"
type: pull_request
state: closed
author: Lee-W
labels: []
assignees: []
draft: true
base: main
head: extend-AIR301-pos-argument
created_at: 2025-10-27T13:14:02Z
updated_at: 2025-12-15T09:49:43Z
url: https://github.com/astral-sh/ruff/pull/21096
synced_at: 2026-01-12T15:57:16Z
```

# [`airflow`] passing positional argument into `airflow.lineage.hook.HookLineageCollector.create_asset` is not allowed (`AIR301`)

---

_@Lee-W_

…e_asset has positional arguments

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

* passing positional argument into `airflow.lineage.hook.HookLineageCollector.create_asset` is not allowed
    * check whether positional argument with 0 index can be found 
## Test Plan

<!-- How was it tested? -->

update the test fixture accordingly and reorganize in the next commit


---

_Comment by @github-actions[bot] on 2025-10-27 13:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "feat(AIR301): warn if airflow.lineage.hook.HookLineageCollector.creat…" to "[`airflow`] passing positional argument into `airflow.lineage.hook.HookLineageCollector.create_asset` is not allowed (`AIR301`)" by @Lee-W on 2025-10-28 09:43_

---

_Marked ready for review by @Lee-W on 2025-10-28 09:45_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR301_AIR301_class_attribute.py.snap`:420 on 2025-10-28 19:19_

Is this the intended message you want to show to users? Shouldn't it say instead that position arguments are disallowed?

---

_@MichaReiser reviewed on 2025-10-28 19:20_

I haven't followed the airflow PRs closely so I don't know if this is the first check for positional arguments. 

Did airflow 2 allow position arguments but airflow 3 does no more?

I can see how this check is useful but it's something a type checker could catch too.

---

_@Lee-W reviewed on 2025-10-29 01:12_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR301_AIR301_class_attribute.py.snap`:420 on 2025-10-29 01:12_

Let me reword it again

---

_Comment by @Lee-W on 2025-10-29 01:13_


> I haven't followed the airflow PRs closely so I don't know if this is the first check for positional arguments.

Yes, I think this is the first one

> Did airflow 2 allow position arguments but airflow 3 does no more?

Yes.

> I can see how this check is useful but it's something a type checker could catch too.

Yes, but we think it would still be helpful if these rules can also detect it here. As most of our users would be data engineers and might not care type checking that much

https://github.com/apache/airflow/issues/47615#issuecomment-2720143869

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR301_AIR301_class_attribute.py.snap`:420 on 2025-10-29 01:24_

After reading the whole message yet again, we should probably add a AIR303 for function signature change. I have another local branch working on something similar and moving this one there might be better as `create_asset` is not removed. Only its signature changed.

WDTY?

---

_@Lee-W reviewed on 2025-10-29 01:24_

---

_Converted to draft by @Lee-W on 2025-10-29 01:24_

---

_@MichaReiser reviewed on 2025-10-30 13:44_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR301_AIR301_class_attribute.py.snap`:420 on 2025-10-30 13:44_

Agree, AIR301 feels out of place for a signature change because it's not a removal

---

_Comment by @MichaReiser on 2025-10-30 13:45_

> Yes, but we think it would still be helpful if these rules can also detect it here. As most of our users would be data engineers and might not care type checking that much

Fair enough. I'd prefer a separate rule because that would also make it easier to deprecate said rule when we introduce a general-purpose rule that detects positional arguments that should have been passed as keyword arguments (which we already have in ty)

---

_Comment by @MichaReiser on 2025-12-10 17:37_

Thanks for working on this PR. 

I'll close this PR as it seems stale. @Lee-W, don't hesitate to submit a new PR if you plan to get back to this. 

---

_Closed by @MichaReiser on 2025-12-10 17:37_

---

_Comment by @sjyangkevin on 2025-12-14 04:08_

thank you @Lee-W for working on this, and thank you @MichaReiser for sharing the feedback. I discussed with @Lee-W , and I would like to pick up the work for adding this rule. As I am new to ruff, so I am still reviewing the contribution guide, and would like to start by asking some basic questions.

### Create a new code AIR303
I am wondering if I need to open an issue to discuss the creation of the code AIR303 before starting the implementation. Have we decided on the naming for the new rule (e.g., detect_positional_argument, ...)?

### High-level thinking on implementation
From my high-level understanding, the new code could be added to the `code.rs`, and then I need to create a new file under `crates/ruff_linter/src/rules/airflow/rules` to implement the rule, and link it with the new code. I am looking into the code change in this PR and trying to understand the process (https://docs.astral.sh/ruff/contributing/#example-adding-a-new-lint-rule) to get a deeper understanding on the details.

https://github.com/astral-sh/ruff/blob/c7eea1f2e33ba67480d4367a45559bf8f2bd1e49/crates/ruff_linter/src/codes.rs#L1119-L1124

Thanks,

---

_Comment by @Lee-W on 2025-12-15 09:49_

> I am wondering if I need to open an issue to discuss the creation of the code AIR303 before starting the implementation.

I think we're good? given that I didn't create an issue for this one.

> Have we decided on the naming for the new rule (e.g., detect_positional_argument, ...)?

function_signature_change or something similar might be better

---
