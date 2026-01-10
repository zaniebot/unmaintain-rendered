```yaml
number: 11179
title: "`red-knot`: introduce a `StatisticsRecorder` trait for the `KeyValueCache`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: redknot-trait
created_at: 2024-04-27T16:38:38Z
updated_at: 2024-04-30T06:14:12Z
url: https://github.com/astral-sh/ruff/pull/11179
synced_at: 2026-01-10T22:37:02Z
```

# `red-knot`: introduce a `StatisticsRecorder` trait for the `KeyValueCache`

---

_Pull request opened by @AlexWaygood on 2024-04-27 16:38_

## Summary

This PR changes the `DebugStatistics` and `ReleaseStatistics` structs so that they implement a common `StatisticsRecorder` trait, and makes the `KeyValueCache` struct generic over a type parameter bound to that trait. The advantage of this approach is that it's much harder for the `DebugStatistics` and `ReleaseStatistics` structs to accidentally grow out of sync in the methods that they implement, which was the cause of the release-build failure recently fixed in #11177.

## Test Plan

`cargo test -p red_knot` and `cargo build --release` both continue to pass for me locally


---

_Review requested from @carljm by @AlexWaygood on 2024-04-27 16:38_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-04-27 16:38_

---

_Label `internal` added by @AlexWaygood on 2024-04-27 16:39_

---

_@MichaReiser reviewed on 2024-04-27 16:46_

---

_Review comment by @MichaReiser on `crates/red_knot/src/cache.rs`:16 on 2024-04-27 16:46_

I like the trait, but making `KeyValueCache` generic over `S` feels heavyweight because it increases the complexity of using the type (yes, it defaults to a type, but you still get scared away by three generics when looking at the type). 


---

_Comment by @github-actions[bot] on 2024-04-27 16:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-04-27 16:57_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/cache.rs`:16 on 2024-04-27 16:57_

Makes sense. Is there a way of using a trait here without making `KeyValueCache` generic? If not, I'm not wedded to this approach -- happy to do something else with this, or not do anything at all!

---

_Review comment by @AlexWaygood on `crates/red_knot/src/cache.rs`:16 on 2024-04-27 16:58_

I suppose I could do `statistics: Box<dyn StatisticsRecorder>`?

---

_@AlexWaygood reviewed on 2024-04-27 16:58_

---

_@MichaReiser reviewed on 2024-04-27 17:13_

---

_Review comment by @MichaReiser on `crates/red_knot/src/cache.rs`:16 on 2024-04-27 17:13_

You could, but that requires an allocation and adds an overhead to cache lookups (even when it's the release statistics). 

I think we should not over-engineer this. We might even go with always having statistics. This is just some prototype code. 

---

_@AlexWaygood reviewed on 2024-04-27 17:15_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/cache.rs`:16 on 2024-04-27 17:15_

Makes sense. Should I just close this for now, then? :-)

---

_@MichaReiser reviewed on 2024-04-27 17:33_

---

_Review comment by @MichaReiser on `crates/red_knot/src/cache.rs`:16 on 2024-04-27 17:33_

I like the `trait`. I would just remove the type param.

---

_@AlexWaygood reviewed on 2024-04-27 17:34_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/cache.rs`:16 on 2024-04-27 17:34_

Ohh, I think I see what you mean. Sorry for being dense.

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-04-27 17:40_

---

_Review request for @carljm removed by @carljm on 2024-04-29 23:49_

---

_Comment by @carljm on 2024-04-29 23:49_

Going to let @MichaReiser confirm if this looks ok.

---

_@MichaReiser approved on 2024-04-30 06:06_

---

_Merged by @AlexWaygood on 2024-04-30 06:14_

---

_Closed by @AlexWaygood on 2024-04-30 06:14_

---

_Branch deleted on 2024-04-30 06:14_

---
