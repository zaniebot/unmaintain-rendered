```yaml
number: 2233
title: "Try eliminating `~AlwaysFalsy` and `~AlwaysTruthy` from intersections if they don't immediately simplify out"
type: issue
state: open
author: carljm
labels: []
assignees: []
created_at: 2025-12-27T00:27:23Z
updated_at: 2025-12-27T00:27:23Z
url: https://github.com/astral-sh/ty/issues/2233
synced_at: 2026-01-12T15:54:26Z
```

# Try eliminating `~AlwaysFalsy` and `~AlwaysTruthy` from intersections if they don't immediately simplify out

---

_@carljm_

They are rarely useful (this is a hypothesis -- to be confirmed or refuted by ecosystem tests), they make our displayed types more complex/confusing, and their tendency to preserve intersections instead of simpler types can cause other issues (e.g. most recently https://github.com/astral-sh/ty/issues/2200). Simplifying them out may improve performance due to more simple types instead of intersections (again a hypothesis to check on an actual PR.)

The easy approach here (good for a prototype PR to check the performance and ecosystem impact) would be to just strip them out in `IntersectionBuilder::build`.

If we decide we want to do this, the better approach might be to remove them entirely as `Type` variants, and replace with `Type::remove_always_truthy` and `Type::remove_always_falsy` transformers. (This will be easier if we update our narrowing support so that it can just return a new type, instead of only a type to intersect with.)

---

_Added to milestone `Stable` by @carljm on 2025-12-27 00:27_

---
