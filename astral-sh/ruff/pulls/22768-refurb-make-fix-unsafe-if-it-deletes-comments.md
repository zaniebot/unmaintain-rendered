```yaml
number: 22768
title: "[`refurb`] Make fix unsafe if it deletes comments (`FURB110`)"
type: pull_request
state: open
author: chirizxc
labels:
  - fixes
assignees: []
base: main
head: FURB110
created_at: 2026-01-20T17:51:10Z
updated_at: 2026-01-21T19:22:33Z
url: https://github.com/astral-sh/ruff/pull/22768
synced_at: 2026-01-21T20:04:11Z
```

# [`refurb`] Make fix unsafe if it deletes comments (`FURB110`)

---

_@chirizxc_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @astral-sh-bot[bot] on 2026-01-20 18:04_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `fixes` added by @ntBre on 2026-01-21 16:47_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/if_exp_instead_of_or_operator.rs`:39 on 2026-01-21 16:48_

Small nit, but the other part of the sentence refers to the `body` of the expression, so I think it's confusing for the `it` here to refer to the fix instead.


```suggestion
/// `if` expression contains side effects or comments.
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB110_FURB110.py.snap`:116 on 2026-01-21 16:50_

I think the check may be too aggressive in this case, if I'm reading the diff correctly. It looks like the contents of the parentheses aren't actually modified here, so the comment is preserved.

---

_@ntBre requested changes on 2026-01-21 16:51_

Thanks! I think the range check is too aggressive in some cases here. We should make sure comments are actually being removed before marking a fix unsafe.

---

_@chirizxc reviewed on 2026-01-21 19:16_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB110_FURB110.py.snap`:116 on 2026-01-21 19:16_

```python
x, y = 1, 2

z = (
    # 1
    x) if (  # 2
    x
) else (
    # Test for y.
    y
)
```
In examples like these, there will also be deletions.

---

_@chirizxc reviewed on 2026-01-21 19:17_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB110_FURB110.py.snap`:116 on 2026-01-21 19:17_

I'm not sure what the right thing to do is in this case. The current rule may limit some cases, but at least there won't be any deletions in save fixes. 

---
