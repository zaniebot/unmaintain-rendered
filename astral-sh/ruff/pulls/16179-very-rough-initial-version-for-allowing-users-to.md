```yaml
number: 16179
title: Very rough initial version for allowing users to make certain specified optional arguments mandatory
type: pull_request
state: closed
author: andyscho
labels: []
assignees: []
draft: true
base: main
head: optional-made-mandatory
created_at: 2025-02-16T08:02:16Z
updated_at: 2025-03-14T09:21:25Z
url: https://github.com/astral-sh/ruff/pull/16179
synced_at: 2026-01-10T19:49:01Z
```

# Very rough initial version for allowing users to make certain specified optional arguments mandatory

---

_Pull request opened by @andyscho on 2025-02-16 08:02_

## Summary

A quick and extremely rough draft example of a potential initial version of #15225.

This draft is not at all complete. It handles a few common cases, but does not work with a lot of other common scenarios and is not very generalizable. 

A better resolution should likely wait on type inference, and possibly also #1774.

## Test Plan

Added a test. This rule does not affect users unless they have manually added settings for it.

---

_Renamed from "Rough initial version for allowing users to make optional arguments mandatory" to "Very rough initial version for allowing users to make optional arguments mandatory" by @andyscho on 2025-02-16 08:02_

---

_Renamed from "Very rough initial version for allowing users to make optional arguments mandatory" to "Very rough initial version for allowing users to make certain specified optional arguments mandatory" by @andyscho on 2025-02-16 08:08_

---

_Comment by @github-actions[bot] on 2025-02-16 08:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-03-14 09:21_

I'll close this due to inactivity. Feel free to re-open a new PR if you get back to it

---

_Closed by @MichaReiser on 2025-03-14 09:21_

---
