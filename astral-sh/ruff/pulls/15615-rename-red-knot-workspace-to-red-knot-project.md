```yaml
number: 15615
title: "Rename `red_knot_workspace` to `red_knot_project`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/workspace-rename
created_at: 2025-01-20T12:26:39Z
updated_at: 2025-01-20T13:02:46Z
url: https://github.com/astral-sh/ruff/pull/15615
synced_at: 2026-01-12T15:55:51Z
```

# Rename `red_knot_workspace` to `red_knot_project`

---

_@MichaReiser_

## Summary

Rename the `red_knot_workspace` crate to `red_knot_project` after renaming `Workspace` to `Project`. 

There's no functional change. I also intentionally am not changing the inner crate structure. I'll do this in a follow up pr



---

_Review requested from @carljm by @MichaReiser on 2025-01-20 12:26_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-20 12:26_

---

_Review requested from @sharkdp by @MichaReiser on 2025-01-20 12:26_

---

_Label `internal` added by @MichaReiser on 2025-01-20 12:26_

---

_Label `red-knot` added by @MichaReiser on 2025-01-20 12:26_

---

_Comment by @github-actions[bot] on 2025-01-20 12:35_

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

_Comment by @AlexWaygood on 2025-01-20 12:51_

This line in the `init-fuzzer` script still references the old name of the crate: https://github.com/astral-sh/ruff/blob/80345e72c485be81c99361c0497aeae43b4f25ef/fuzz/init-fuzzer.sh#L26

---

_Comment by @AlexWaygood on 2025-01-20 12:54_

I can also see a few references in snapshots but they don't seem to be causing tests to fail, so maybe it doesn't matter?

```
~/dev/ruff (micha/workspace-rename)⚡ % git grep red_knot_workspace
crates/red_knot_project/src/project/snapshots/red_knot_project__project__metadata__tests__nested_projects_in_root_project.snap:source: crates/red_knot_workspace/src/project/metadata.rs
crates/red_knot_project/src/project/snapshots/red_knot_project__project__metadata__tests__nested_projects_in_sub_project.snap:source: crates/red_knot_workspace/src/project/metadata.rs
crates/red_knot_project/src/project/snapshots/red_knot_project__project__metadata__tests__nested_projects_with_outer_knot_section.snap:source: crates/red_knot_workspace/src/project/metadata.rs
crates/red_knot_project/src/project/snapshots/red_knot_project__project__metadata__tests__nested_projects_without_knot_sections.snap:source: crates/red_knot_workspace/src/project/metadata.rs
crates/red_knot_project/src/project/snapshots/red_knot_project__project__metadata__tests__project_with_knot_and_pyproject_toml.snap:source: crates/red_knot_workspace/src/project/metadata.rs
crates/red_knot_project/src/project/snapshots/red_knot_project__project__metadata__tests__project_with_pyproject.snap:source: crates/red_knot_workspace/src/project/metadata.rs
crates/red_knot_project/src/project/snapshots/red_knot_project__project__metadata__tests__project_without_pyproject.snap:source: crates/red_knot_workspace/src/project/metadata.rs
```

---

_Comment by @MichaReiser on 2025-01-20 12:56_

> I can also see a few references in snapshots but they don't seem to be causing tests to fail, so maybe it doesn't matter?
> 
> ```
> ~/dev/ruff (micha/workspace-rename)⚡ % git grep red_knot_workspace
> crates/red_knot_project/src/project/snapshots/red_knot_project__project__metadata__tests__nested_projects_in_root_project.snap:source: crates/red_knot_workspace/src/project/metadata.rs
> crates/red_knot_project/src/project/snapshots/red_knot_project__project__metadata__tests__nested_projects_in_sub_project.snap:source: crates/red_knot_workspace/src/project/metadata.rs
> crates/red_knot_project/src/project/snapshots/red_knot_project__project__metadata__tests__nested_projects_with_outer_knot_section.snap:source: crates/red_knot_workspace/src/project/metadata.rs
> crates/red_knot_project/src/project/snapshots/red_knot_project__project__metadata__tests__nested_projects_without_knot_sections.snap:source: crates/red_knot_workspace/src/project/metadata.rs
> crates/red_knot_project/src/project/snapshots/red_knot_project__project__metadata__tests__project_with_knot_and_pyproject_toml.snap:source: crates/red_knot_workspace/src/project/metadata.rs
> crates/red_knot_project/src/project/snapshots/red_knot_project__project__metadata__tests__project_with_pyproject.snap:source: crates/red_knot_workspace/src/project/metadata.rs
> crates/red_knot_project/src/project/snapshots/red_knot_project__project__metadata__tests__project_without_pyproject.snap:source: crates/red_knot_workspace/src/project/metadata.rs
> ```

Yeah, they are just informational. 

---

_@AlexWaygood approved on 2025-01-20 12:58_

---

_Comment by @MichaReiser on 2025-01-20 13:00_

I rename the tests in my next PR (where I moved the files again, so the path needs updating anyway)

---

_Merged by @MichaReiser on 2025-01-20 13:02_

---

_Closed by @MichaReiser on 2025-01-20 13:02_

---

_Branch deleted on 2025-01-20 13:02_

---
