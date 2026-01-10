```yaml
number: 15773
title: "[red-knot] Decompose bool to Literal[True, False] in unions and intersections (v2)"
type: pull_request
state: open
author: AlexWaygood
labels:
  - ty
assignees: []
draft: true
base: main
head: alex/truthy-unions-6
created_at: 2025-01-27T17:10:58Z
updated_at: 2025-01-27T17:19:38Z
url: https://github.com/astral-sh/ruff/pull/15773
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Decompose bool to Literal[True, False] in unions and intersections (v2)

---

_Pull request opened by @AlexWaygood on 2025-01-27 17:10_

## Summary

An alternative to https://github.com/astral-sh/ruff/pull/15738, which might be slightly more readable and easier to reason about. I'm also curious to see how this compares with the Codspeed benchmarks.

## Test Plan

Same tests as https://github.com/astral-sh/ruff/pull/15738


---

_Label `red-knot` added by @AlexWaygood on 2025-01-27 17:10_

---

_Comment by @AlexWaygood on 2025-01-27 17:19_

@sharkdp -- curious if you find this more or less easy to understand than https://github.com/astral-sh/ruff/pull/15738? The final commit here is the only difference between the two. (I'll add a comment to the branch about boolean literals delegating to `KnownClass::Int` whichever of the two we decide we prefer for readability.)

Performance seems to be basically the same between the two. Maybe this one is a tiny bit slower? But it could just be noise.

---
