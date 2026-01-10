```yaml
number: 14989
title: Approximate SemanticIndex hash maps
type: pull_request
state: closed
author: MichaReiser
labels: []
assignees: []
draft: true
base: main
head: micha/perf-approximate-hashmaps
created_at: 2024-12-15T18:02:38Z
updated_at: 2024-12-15T18:09:35Z
url: https://github.com/astral-sh/ruff/pull/14989
synced_at: 2026-01-10T20:42:27Z
```

# Approximate SemanticIndex hash maps

---

_Pull request opened by @MichaReiser on 2024-12-15 18:02_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-12-15 18:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-12-15 18:09_

Hmm, I see like a 2% perf improvement locally but that's probably also not worth it. The idea was to approximate the hash map sizes in `SemanticIndex` to avoid costly resizes. 

---

_Closed by @MichaReiser on 2024-12-15 18:09_

---
