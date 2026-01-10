```yaml
number: 19110
title: "[`flake8-simplify`] Make example error out-of-the-box (`SIM401`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-7
created_at: 2025-07-03T00:26:11Z
updated_at: 2025-07-03T14:15:31Z
url: https://github.com/astral-sh/ruff/pull/19110
synced_at: 2026-01-10T18:33:12Z
```

# [`flake8-simplify`] Make example error out-of-the-box (`SIM401`)

---

_Pull request opened by @MeGaGiGaGon on 2025-07-03 00:26_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #18972

This PR makes [enumerate-for-loop [if-else-block-instead-of-dict-get (SIM401)](https://docs.astral.sh/ruff/rules/if-else-block-instead-of-dict-get/#if-else-block-instead-of-dict-get-sim401)'s example error out-of-the-box

[Old example](https://play.ruff.rs/635629eb-7146-45a8-9e0c-4a0aa9446ded)
```py
if "bar" in foo:
    value = foo["bar"]
else:
    value = 0
```

[New example](https://play.ruff.rs/a1227ec9-05c2-4a22-800d-c76cb7abe249)
```py
foo = {}
if "bar" in foo:
    value = foo["bar"]
else:
    value = 0
```

The "Use instead" section was also updated similarly.

The docs for `SIM401` also has another section on the preview ternary version, but it does not seem to check that the variable is a dict (bug?) https://play.ruff.rs/c0feada8-a7fe-43f7-b57e-c10520fdcdca

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-07-03 00:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-07-03 13:59_

Thanks!

Yeah, I think the if-expression version is missing this check:

https://github.com/astral-sh/ruff/blob/bdbeab81669096c4e6359df071187e3173f46d59/crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_dict_get.rs#L135-L140

Do you want to open a follow-up issue or PR for that? Good catch!

---

_Label `documentation` added by @ntBre on 2025-07-03 14:00_

---

_Merged by @ntBre on 2025-07-03 14:00_

---

_Closed by @ntBre on 2025-07-03 14:00_

---

_Branch deleted on 2025-07-03 14:15_

---
