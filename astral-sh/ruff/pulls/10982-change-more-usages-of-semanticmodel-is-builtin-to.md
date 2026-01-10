```yaml
number: 10982
title: "Change more usages of `SemanticModel::is_builtin` to use `resolve_builtin_symbol` or `match_builtin_expr`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: builtins-3
created_at: 2024-04-16T20:43:36Z
updated_at: 2024-04-17T06:50:14Z
url: https://github.com/astral-sh/ruff/pull/10982
synced_at: 2026-01-10T22:37:01Z
```

# Change more usages of `SemanticModel::is_builtin` to use `resolve_builtin_symbol` or `match_builtin_expr`

---

_Pull request opened by @AlexWaygood on 2024-04-16 20:43_

## Summary

This PR switches more callsites of `SemanticModel::is_builtin` to move over to the new methods I introduced in #10919, which are more concise and more accurate. I missed these calls in the first PR.

## Test Plan

`cargo test`. I haven't introduced any new tests in this PR, because I added quite a few in #10919, and this is just doing the same thing as that PR. One existing violation in the test fixtures becomes newly fixable, though.

---

_Label `internal` added by @AlexWaygood on 2024-04-16 20:43_

---

_Comment by @AlexWaygood on 2024-04-16 20:44_

I've added the `internal` label because we don't need two changelog entries for https://github.com/astral-sh/ruff/pull/10919 _and_ this.

---

_Comment by @AlexWaygood on 2024-04-16 21:10_

Performance is [overall neutral](https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:builtins-3), but some linter benchmarks are showing regressions of 1-2%. I think that's _probably_ okay, given that #10982 was also overall neutral, and the final version of that PR had improvements of 2-3% on some of the same benchmarks. I can't see an easy/clean way of moving the calls to the semantic model lower down in any of the rules I'm touching here.

---

_Comment by @github-actions[bot] on 2024-04-16 21:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-04-17 06:43_

---

_Merged by @AlexWaygood on 2024-04-17 06:50_

---

_Closed by @AlexWaygood on 2024-04-17 06:50_

---

_Branch deleted on 2024-04-17 06:50_

---
