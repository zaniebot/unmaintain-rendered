```yaml
number: 12626
title: Fix file watcher stop data race
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-watcher-stop-race
created_at: 2024-08-02T12:06:21Z
updated_at: 2024-08-02T17:02:51Z
url: https://github.com/astral-sh/ruff/pull/12626
synced_at: 2026-01-10T21:47:02Z
```

# Fix file watcher stop data race

---

_Pull request opened by @MichaReiser on 2024-08-02 12:06_

## Summary

This PR fixes a bug where stopping the file watcher could lead to a data race (and panic)
where the debouncer thread already stopped but the notify callback tried to send one more change event
that then failed because the corresponding receiver for the sender is gone. 

https://github.com/astral-sh/ruff/actions/runs/10214844462/job/28263055374?pr=12538

```
thread 'notify-rs inotify loop' panicked at /home/runner/work/ruff/ruff/crates/red_knot_workspace/src/watch/watcher.rs:86:86:
called `Result::unwrap()` on an `Err` value: "SendError(..)"
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
thread 'changed_file' panicked at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/notify-6.1.1/src/inotify.rs:568:51:
called `Result::unwrap()` on an `Err` value: "SendError(..)"
```

This PR fixes the race by removing the `Stop` message and instead rely on the precense of a 
sender to determine for how long the receiver should run. This guarantees that sending
should always succeed in the notify callback. 



## Test Plan

CI? Not sure how to write a test for this. 


---

_Review requested from @carljm by @MichaReiser on 2024-08-02 12:06_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-02 12:06_

---

_Label `red-knot` added by @MichaReiser on 2024-08-02 12:06_

---

_Comment by @AlexWaygood on 2024-08-02 12:10_

I'll let Carl review this one; I think he's more familiar with this code than me ;)

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2024-08-02 12:10_

---

_Comment by @github-actions[bot] on 2024-08-02 12:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-08-02 16:38_

---

_Merged by @MichaReiser on 2024-08-02 17:02_

---

_Closed by @MichaReiser on 2024-08-02 17:02_

---

_Branch deleted on 2024-08-02 17:02_

---
