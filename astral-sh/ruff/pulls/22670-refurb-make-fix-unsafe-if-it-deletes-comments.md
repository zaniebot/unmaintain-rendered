```yaml
number: 22670
title: "[`refurb`] Make fix unsafe if it deletes comments (`FURB145`)"
type: pull_request
state: open
author: chirizxc
labels: []
assignees: []
base: main
head: FURB145
created_at: 2026-01-17T21:56:31Z
updated_at: 2026-01-18T10:53:00Z
url: https://github.com/astral-sh/ruff/pull/22670
synced_at: 2026-01-18T11:18:27Z
```

# [`refurb`] Make fix unsafe if it deletes comments (`FURB145`)

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

_Comment by @astral-sh-bot[bot] on 2026-01-17 22:08_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@MichaReiser reviewed on 2026-01-18 10:53_

Thank you. 

We should also update the documentation to mention that the fix is unsafe if there's a comment (I don't think it's necessary to add a code example, a short sentence should be sufficient)

https://github.com/astral-sh/ruff/blob/b4b8299d6cc3db6fd6125a30d58d58ef3d3069bf/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_dict_comprehension_for_iterable.rs#L36-L38

---
