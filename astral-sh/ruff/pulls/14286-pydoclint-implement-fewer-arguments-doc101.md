```yaml
number: 14286
title: "[`pydoclint`] Implement `fewer arguments` (`DOC101`)"
type: pull_request
state: closed
author: jamesfricker
labels: []
assignees: []
draft: true
base: main
head: feat/pydoclint-doc101
created_at: 2024-11-11T21:47:06Z
updated_at: 2024-11-12T10:47:07Z
url: https://github.com/astral-sh/ruff/pull/14286
synced_at: 2026-01-12T15:55:47Z
```

# [`pydoclint`] Implement `fewer arguments` (`DOC101`)

---

_@jamesfricker_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Implement DOC101 for pydoclint

https://jsh9.github.io/pydoclint/violation_codes.html#1-doc1xx-violations-about-input-arguments

(First time ruff contribution!)

Since this is a new rule, I'm probably missing some documentation somewhere. 

## Test Plan

<!-- How was it tested? -->

Unit tests. 


---

_Renamed from "doc101" to "[`pydoclint`] Implement `fewer arguments` (`DOC101`)" by @jamesfricker on 2024-11-11 21:52_

---

_Comment by @github-actions[bot] on 2024-11-11 22:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @jamesfricker on 2024-11-12 10:47_

turns out that
- I didn't do this right because the snapshots should have some more info
- it's a duplicate of a better pr (https://github.com/astral-sh/ruff/pull/13280)

closing 😁

---

_Closed by @jamesfricker on 2024-11-12 10:47_

---

_Branch deleted on 2024-11-12 10:47_

---
