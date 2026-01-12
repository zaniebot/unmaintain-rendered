```yaml
number: 17707
title: Add AIR301 rule
type: pull_request
state: merged
author: prabhusneha
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add_AIR301_rule
created_at: 2025-04-29T14:09:48Z
updated_at: 2025-08-11T13:14:43Z
url: https://github.com/astral-sh/ruff/pull/17707
synced_at: 2026-01-12T15:56:04Z
```

# Add AIR301 rule

---

_@prabhusneha_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add "airflow.secrets.cache.SecretCache" → "airflow.sdk.cache.SecretCache" rule

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @prabhusneha on 2025-04-29 14:10_

cc: @Lee-W 

---

_@ntBre reviewed on 2025-04-29 21:53_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/airflow/AIR301_names.py`:61 on 2025-04-29 21:53_

I'm not seeing this in any of the snapshots, is that expected?

---

_@ntBre reviewed on 2025-04-29 21:54_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/airflow/AIR301_names.py`:61 on 2025-04-29 21:54_

Oh sorry, didn't realize it was still a draft!

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:685 on 2025-04-30 01:03_

```suggestion
        ["airflow", "secrets", "cache", "SecretCache"] => Replacement::AutoImport {
```

---

_@Lee-W reviewed on 2025-04-30 01:04_

---

_Review comment by @Lee-W on `crates/ruff_linter/resources/test/fixtures/airflow/AIR301_names.py`:61 on 2025-04-30 01:04_

Nope, just left a comment on it. 

We're inviting new friends to work with the ruff airflow rules 🙂 

---

_@Lee-W reviewed on 2025-04-30 01:04_

---

_@prabhusneha reviewed on 2025-04-30 05:44_

---

_Review comment by @prabhusneha on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:685 on 2025-04-30 05:44_

Ah, fixed. Thanks Wei

---

_@prabhusneha reviewed on 2025-04-30 05:46_

---

_Review comment by @prabhusneha on `crates/ruff_linter/resources/test/fixtures/airflow/AIR301_names.py`:61 on 2025-04-30 05:46_

Fixed it

---

_Marked ready for review by @prabhusneha on 2025-04-30 05:46_

---

_Label `rule` added by @MichaReiser on 2025-04-30 06:25_

---

_Label `preview` added by @MichaReiser on 2025-04-30 06:25_

---

_Review requested from @ntBre by @MichaReiser on 2025-04-30 06:25_

---

_Comment by @github-actions[bot] on 2025-04-30 06:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@Lee-W reviewed on 2025-04-30 06:41_

This is not in Airflow `3.0.0` but is expected to be released in 3.0.1. @ntBre is there any suggestion on this case? Should we create a new error? Thanks!

---

_@ntBre approved on 2025-04-30 20:02_

---

_Comment by @ntBre on 2025-04-30 20:04_

I think if you want people to be able to enable this separately from the other 3.0 rules, a separate rule would be required. Alternatively, we could hold off on adding this until 3.0.1 is released, if that would work for you all.

Which part is not in 3.0.0? If the replacement is available but the old version isn't deleted, I think it could make sense just to enable the rule anyway, but that obviously wouldn't make sense if the replacement is only added in 3.0.1.

I'll go ahead and review but hold off merging in case you want a separate rule.

---

_Comment by @dhruvmanila on 2025-04-30 21:03_

I think I'd prefer to keep it in the same rule, you could update the diagnostic message for the existing rules to make it explicit that this "requires Airflow 3.0 or greater". It also seems to be something that's part of a patch version in Airflow so I'm assuming that it's backwards compatible and as Ruff can't detect the dependency version it makes sense that Ruff assumes that users are on the latest version of Airflow 3. I think it's the same for NumPy 2.0 deprecation rules if I'm not mistaken.

---

_Comment by @Lee-W on 2025-05-06 10:03_

>  we could hold off on adding this until 3.0.1 is released,

Yep, I think we should hold off till 3.0.1

> Which part is not in 3.0.0? 

The replacement itself. It will be introduced in 3.0.1

> you could update the diagnostic message for the existing rules to make it explicit that this "requires Airflow 3.0 or greater".

Yep, I was thinking of something similar. Cool! I'll go with this route after 3.0.1 release. Thanks!

---

_Comment by @ntBre on 2025-05-06 13:11_

I'll mark this as a draft until we're ready to merge, which sounds like after 3.0.1 is released. Feel free to ping me whenever it's ready!

---

_Converted to draft by @ntBre on 2025-05-06 13:11_

---

_Comment by @MichaReiser on 2025-07-10 14:13_

@Lee-W what's the status on this rule? Can we close this PR?

---

_Comment by @Lee-W on 2025-07-18 14:33_

Hey, sorry for the late reply. I'll take a look tomorrow

---

_Comment by @Lee-W on 2025-07-20 20:14_

I think this is needed and I'll take over once I'm back from my vacaction

---

_Comment by @MichaReiser on 2025-07-21 06:55_

Enjoy your vacation

---

_Comment by @Lee-W on 2025-08-01 11:12_

The PR is now ready, but I'm not allowed to change anything other than code somehow 🤔 

---

_Marked ready for review by @prabhusneha on 2025-08-01 11:15_

---

_@ntBre approved on 2025-08-08 19:00_

Still looks good to me, thank you! I think you'll just need to resolve the conflicts and then we can merge.

---

_Merged by @ntBre on 2025-08-11 13:14_

---

_Closed by @ntBre on 2025-08-11 13:14_

---
