```yaml
number: 21429
title: "Use `profiling` profile for `cargo test(linux, release)`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: micha/test-release-profiling
created_at: 2025-11-13T13:51:39Z
updated_at: 2025-11-13T15:27:38Z
url: https://github.com/astral-sh/ruff/pull/21429
synced_at: 2026-01-12T15:57:23Z
```

# Use `profiling` profile for `cargo test(linux, release)`

---

_@MichaReiser_

## Summary

Use the `profiling` profile over the `release` profile for `cargo test(linux, release)`. 

The idea of the `release` CI job is to catch bugs related to when `debug_assertions` is disabled. 
Since testing ruff/ty with `debug_assertions` doesn't require the `releaes` profile and because the `release` profile takes very long to build,  this PR switches to `profiling` to accomplish the same in less time.

I had to switch to use `nextest` directly because `cargo insta` doesn't remap the `--profile profiling` argument  to `--cargo-profile profiling` as required by insta. The only consequence this has is that we need to run the doctests ourselves and that we don't get unused snapshot detection. I don't think the latter is important as we already enforce this in all our other test CI jobs and is only really important to find stale snapshots.


## Test Plan

The release job now completes in 4min 30s (with a cold cache) compared to 9min before.


---

_Label `ci` added by @MichaReiser on 2025-11-13 13:51_

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 14:21_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Marked ready for review by @MichaReiser on 2025-11-13 14:34_

---

_@ntBre approved on 2025-11-13 15:25_

---

_Merged by @MichaReiser on 2025-11-13 15:27_

---

_Closed by @MichaReiser on 2025-11-13 15:27_

---

_Branch deleted on 2025-11-13 15:27_

---
