```yaml
number: 15472
title: Remove workspace support
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/workspace-project
created_at: 2025-01-14T11:06:45Z
updated_at: 2025-01-15T08:03:40Z
url: https://github.com/astral-sh/ruff/pull/15472
synced_at: 2026-01-12T15:55:51Z
```

# Remove workspace support

---

_@MichaReiser_

## Summary

This PR removes Red Knot's native support for workspaces. This doesn't mean that Red Knot won't support workspaces long-term, but the goal is to build on uv's workspace structure instead of defining its own. I forsee that Red Knot will need to represent the package structure to resolve the closest configuration. However, I don't think it's worth keeping the existing code around as the uv integration is still rather far out. 

* Renames `Workspace` to `Project` (in many places)
* Remove `Package` and the `Project::packages` field
* Removes workspace specific tests. 
* Rename `RootDatabase` to `ProjectDatabase`: It's the database for a single project.

I intentionally didn't rename the `red_knot_workspace` crate as part of this PR to avoid mixing structural and functional changes. 

---

_Label `red-knot` added by @MichaReiser on 2025-01-14 11:06_

---

_Comment by @github-actions[bot] on 2025-01-14 11:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/project.rs`:1 on 2025-01-14 12:27_

It's unfortunate that git doesn't recognize this as the same file as `workspace.rs` 

---

_@MichaReiser reviewed on 2025-01-14 12:27_

---

_@MichaReiser reviewed on 2025-01-14 12:47_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/project/metadata.rs`:1 on 2025-01-14 12:47_

This file is also a trimmed down version of what used to be in `workspace/metadata` but git doesn't recognize that ;( 

---

_@MichaReiser reviewed on 2025-01-14 12:48_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/tests/check.rs`:4 on 2025-01-14 12:48_

The paths here are somewhat funny. `red_knot_project::project`. I need to think about how I can improve that but I'll leave this to another PR as this is a pre-existing issue.

---

_Marked ready for review by @MichaReiser on 2025-01-14 12:51_

---

_Review requested from @carljm by @MichaReiser on 2025-01-14 12:51_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-14 12:51_

---

_Review requested from @sharkdp by @MichaReiser on 2025-01-14 12:51_

---

_@MichaReiser reviewed on 2025-01-14 12:58_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/project/metadata.rs`:60 on 2025-01-14 12:58_

I know this has come up in the CLI proposal. Skipping over projects without a `tool.knot` section is sort of important to support projects using uv's workspace layout where you have

```
root
| - packages
|   | -- a
|   |    | - pyproject.toml
|   |    | - ...
|   | -- a
|   |   | - pyproject.toml
|   |   | - ...
|  - src
|   | - ... 
| - pyproject.toml (with `tool.knot` section)
```


In this case, it's important that the entire workspace can be checked as one. That's why running `knot` in `packages/a` should also pick up the configuration from the root, unless that one package overrides its own configuration. 


---

_Review comment by @carljm on `crates/red_knot_workspace/src/project/metadata.rs`:60 on 2025-01-14 18:51_

I'm still not sure I agree that this is important, or the right behavior, but we can always change it and I think it will be more useful to sort it out later with real use cases.

---

_@carljm approved on 2025-01-14 18:51_

It's a bit hard to find the meaningful changes in this PR, but I think I found most of them, and it looks good to me, thank you!

---

_Comment by @MichaReiser on 2025-01-15 08:03_

> It's a bit hard to find the meaningful changes in this PR, but I think I found most of them, and it looks good to me, thank you!

I'm sorry for that. I only realized when reading your comment that I could have done the rename first and then removed `packages`. I'll try to consider this next time.

---

_Merged by @MichaReiser on 2025-01-15 08:03_

---

_Closed by @MichaReiser on 2025-01-15 08:03_

---

_Branch deleted on 2025-01-15 08:03_

---
