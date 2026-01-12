```yaml
number: 15505
title: "Support `knot.toml` files in project discovery"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/knot-toml
created_at: 2025-01-15T15:42:19Z
updated_at: 2025-01-17T09:04:04Z
url: https://github.com/astral-sh/ruff/pull/15505
synced_at: 2026-01-12T15:55:51Z
```

# Support `knot.toml` files in project discovery

---

_@MichaReiser_

## Summary

This PR adds support for `knot.toml` files during project discovery. 

`knot.toml` files take precedence over any `pyproject.toml` but we still need to read the `pyproject.toml`
to get the project's name and any `requires-python` constraint (not part of this PR). 

This PR doesn't add support for:

* Reading `knot.toml` files from ancestor directories (hierarchical configuration)
* Reading a user configuration

Part of https://github.com/astral-sh/ty/issues/219

## Test Plan

Added unit test


---

_Label `red-knot` added by @MichaReiser on 2025-01-15 15:42_

---

_@MichaReiser reviewed on 2025-01-15 15:44_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/project/metadata.rs`:96 on 2025-01-15 15:44_

`map_err` didn't work here (and below). The borrow checker gets mad at me for consuming `pyproject_path` in the closure and using it further down. 

---

_Comment by @github-actions[bot] on 2025-01-15 15:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @MichaReiser on 2025-01-15 15:57_

---

_Review requested from @carljm by @MichaReiser on 2025-01-15 15:57_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-15 15:57_

---

_Review requested from @sharkdp by @MichaReiser on 2025-01-15 15:57_

---

_Review comment by @carljm on `crates/red_knot_workspace/src/project/metadata.rs`:56 on 2025-01-15 18:29_

```suggestion
    /// Loads a project from a `knot.toml` file.
```

---

_Review comment by @carljm on `crates/red_knot_workspace/src/project/metadata.rs`:57 on 2025-01-15 18:34_

I found this name confusing, because the TOML part all happens in `discover` method, so this method doesn't do anything "from Knot TOML", it's "from root and options and maybe PyProject". (I initially had a comment here assuming this method was just a stub for now and the TOML-parsing part was coming in a later PR, and only realized that was wrong once I got to the `discover` method below.)

I understand now the structure you're going for where `from_pyproject` and `from_knot_toml` are parallel, depending which config file we found. I think those two cases share so much, that I would find it clearer if this method were named something more generic (`from_options`?) and were actually called by `from_pyproject` as well. Then you also wouldn't need the separate `name_from_pyproject` method, it could be inlined here. And similarly you'd save repetition in the future with `requires-python` support.

If you don't like that restructuring, then I think the parallel you are going for would be stronger if the naming was parallel: `from_pyproject_toml` and `from_knot_toml` or else `from_pyproject` and `from_knot`; I think the latter makes more sense because nothing TOML-related happens in these methods, but I understand `from_knot` may not be super clear what it's referring to (which is why my preferred option above is to not go for that parallel at all and just make this a more general constructor.)

But up to you.

---

_@carljm approved on 2025-01-15 18:44_

Nice!

---

_Merged by @MichaReiser on 2025-01-17 09:01_

---

_Closed by @MichaReiser on 2025-01-17 09:01_

---

_Branch deleted on 2025-01-17 09:02_

---
