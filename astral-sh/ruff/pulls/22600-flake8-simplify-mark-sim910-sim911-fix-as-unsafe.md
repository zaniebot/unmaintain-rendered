```yaml
number: 22600
title: "[flake8-simplify] Mark SIM910/SIM911 fix as unsafe when deleting comm…"
type: pull_request
state: open
author: leonace924
labels: []
assignees: []
base: main
head: 18775_sim910_sim911_unsafe_fix_comments
created_at: 2026-01-15T13:55:18Z
updated_at: 2026-01-15T15:17:43Z
url: https://github.com/astral-sh/ruff/pull/22600
synced_at: 2026-01-15T15:50:21Z
```

# [flake8-simplify] Mark SIM910/SIM911 fix as unsafe when deleting comm…

---

_@leonace924_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #18775


<!-- What's the purpose of the change? What does it do, and why? -->

The SIM910 (`dict-get-with-none-default`) and SIM911 (`zip-dict-keys-and-values`) rules were silently deleting comments when applying fixes.

For example, SIM910 would transform:

```python
ages = {"Tom": 23, "Maria": 23, "Dog": 11}
age = ages.get(  # comment
    "Cat", None
)
```
    ...
```

into:

```python
for k, v in d.items():
    ...
```

This fix marks such transformations as **unsafe** when comments would be deleted, requiring users to explicitly opt-in with `--unsafe-fixes`.

## Approach

Uses the established `CommentRanges::intersects()` pattern already used by other rules in the codebase (as suggested by @MichaReiser in the issue). When comments exist within the expression's range, the fix is marked as `Applicability::Unsafe` instead of `Applicability::Safe`.

## Test Plan

<!-- How was it tested? -->

Contribution by Gittensor, see my contribution statistics at https://gittensor.io/miners/details?githubId=42954461


---

_Comment by @leonace924 on 2026-01-15 13:57_

@BurntSushi @MichaReiser would you approve testing and review this PR?
Thank you for your time

---

_Comment by @MichaReiser on 2026-01-15 14:27_

Thanks for your PR

> @BurntSushi @MichaReiser would you approve testing and review this PR? Thank you for your time

Please give us some time before pinging maintainers. We already get a lot of notifications and more notifications doesn't help us to get things done faster. BurntSushi also doesn't work at this part of the code base at all.

---

_Comment by @astral-sh-bot[bot] on 2026-01-15 15:15_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Comment by @leonace924 on 2026-01-15 15:17_

All testing passed!

---
