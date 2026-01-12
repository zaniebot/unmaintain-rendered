```yaml
number: 18685
title: "[ty] Anchor all exclude patterns"
type: pull_request
state: merged
author: MichaReiser
labels:
  - configuration
  - ty
assignees: []
merged: true
base: main
head: micha/exclude-anchoring
created_at: 2025-06-15T13:44:14Z
updated_at: 2025-06-18T08:57:37Z
url: https://github.com/astral-sh/ruff/pull/18685
synced_at: 2026-01-12T15:56:23Z
```

# [ty] Anchor all exclude patterns

---

_@MichaReiser_

## Summary

As [discussed](https://github.com/astral-sh/ruff/pull/18648#discussion_r2145230097), this PR changes the semantics of exclude globs to be anchored. That means, `src` now only matches `<project_root>/src` but no longer `<project_root>/build/src`. Or `*.py` now only matches python files directly inside the project root but no longer matches any file with a `py` extension.

The motivation for this change is to make the behavior consistent with `incldue` where anchoring is desired to allow eagerly skipping over directories that are known not to be included. 

Just for reference: 

* This will make ty's exclude handling slightly more incosistento to uv's build backend exclude globs. The two already differed before in that uv anchors no exclude patterns at all whereas, before this PR, ty only anchored paths containing a `/` (same as gitignore). 
* This deviats from gitignore which only anchors paths that contain a `/` (a path separator)

## Test Plan

Update tests

---

_Label `configuration` added by @MichaReiser on 2025-06-15 13:44_

---

_Label `ty` added by @MichaReiser on 2025-06-15 13:44_

---

_Comment by @github-actions[bot] on 2025-06-15 13:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser reviewed on 2025-06-15 13:49_

---

_Review comment by @MichaReiser on `crates/ty_project/src/glob/portable.rs`:35 on 2025-06-15 13:49_

Sorry, I couldn't help myself but this small refactor allows removing a fair chunk of code and makes the API more explicit

---

_Marked ready for review by @MichaReiser on 2025-06-15 13:52_

---

_Review requested from @carljm by @MichaReiser on 2025-06-15 13:52_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-15 13:52_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-15 13:52_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-15 13:52_

---

_Review request for @carljm removed by @carljm on 2025-06-17 01:56_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-06-17 06:21_

---

_Review comment by @BurntSushi on `crates/ty_project/src/glob/exclude.rs`:104 on 2025-06-17 12:32_

```suggestion
/// The differences with the ignore's crate version are:
```

---

_@BurntSushi approved on 2025-06-17 12:34_

Nice, this seems nicer to me. :-) Thank you!

---

_Merged by @MichaReiser on 2025-06-18 08:57_

---

_Closed by @MichaReiser on 2025-06-18 08:57_

---

_Branch deleted on 2025-06-18 08:57_

---
