```yaml
number: 15784
title: "[red-knot] Ensure that `bool | AlwaysFalsy` is considered equivalent to `Literal[True] | AlwaysFalsy`"
type: pull_request
state: open
author: AlexWaygood
labels:
  - ty
assignees: []
draft: true
base: main
head: alex/truthy-unions-7
created_at: 2025-01-28T12:31:04Z
updated_at: 2025-03-14T09:19:39Z
url: https://github.com/astral-sh/ruff/pull/15784
synced_at: 2026-01-12T15:55:52Z
```

# [red-knot] Ensure that `bool | AlwaysFalsy` is considered equivalent to `Literal[True] | AlwaysFalsy`

---

_@AlexWaygood_

## Summary

This PR is yet another alternative to https://github.com/astral-sh/ruff/pull/15738 and #15773.

I'll writeup a fuller PR summary once I've seen the benchmark results...

## Test Plan

<!-- How was it tested? -->


---

_Label `red-knot` added by @AlexWaygood on 2025-01-28 12:31_

---

_Comment by @MichaReiser on 2025-03-14 09:19_

@AlexWaygood is this PR (and the other once mentioned in the summary) still something you plan to work on or can we close the PRs?

---
