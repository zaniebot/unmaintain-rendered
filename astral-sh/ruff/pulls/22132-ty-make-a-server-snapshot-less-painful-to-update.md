```yaml
number: 22132
title: "[ty] Make a server snapshot less painful to update"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/server-snapshot
created_at: 2025-12-21T20:47:16Z
updated_at: 2025-12-22T12:18:20Z
url: https://github.com/astral-sh/ruff/pull/22132
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Make a server snapshot less painful to update

---

_Pull request opened by @AlexWaygood on 2025-12-21 20:47_

## Summary

Currently this snapshot feels like it causes quite a lot of development pain (at least for me), because I never think to run the `ty_server` tests if all I'm doing is adding a new diagnostic to the `ty_python_semantic` crate. So I always push my PR and only remember that there's this snapshot that also needs to be updated when CI starts failing on my PR.

This PR adds a new filter to the snapshot so that the snapshot no longer lists every rule that ty has, meaning that the snapshot no longer needs to be updated every time we add a new diagnostic to `ty_python_semantic`.

## Test Plan

`cargo test -p ty_server`


---

_Label `testing` added by @AlexWaygood on 2025-12-21 20:47_

---

_Review requested from @carljm by @AlexWaygood on 2025-12-21 20:47_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-12-21 20:47_

---

_Label `ty` added by @AlexWaygood on 2025-12-21 20:47_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-21 20:47_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-21 20:47_

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/commands.rs`:50 on 2025-12-22 08:38_

```suggestion
            (r"rules: \{(.|\n)+?\}\,","rules: <RULES>,"),
```

---

_@MichaReiser approved on 2025-12-22 08:39_

It would be nice to retain at least a few rules to see how the output looks like but that's a bit tricky, I think

---

_Merged by @AlexWaygood on 2025-12-22 12:13_

---

_Closed by @AlexWaygood on 2025-12-22 12:13_

---

_Branch deleted on 2025-12-22 12:13_

---

_Comment by @AlexWaygood on 2025-12-22 12:18_

> It would be nice to retain at least a few rules to see how the output looks like but that's a bit tricky, I think

I guess a way of doing that would be to have a mock rule registry with a stable list of rules, and snapshot the debug command with that mock? But yeah, sounds nontrivial to setup

---
