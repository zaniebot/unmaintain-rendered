```yaml
number: 16227
title: Workspace versioning
type: pull_request
state: closed
author: KGrewal1
labels: []
assignees: []
base: main
head: workspace_version
created_at: 2025-02-18T11:25:13Z
updated_at: 2025-02-18T12:51:54Z
url: https://github.com/astral-sh/ruff/pull/16227
synced_at: 2026-01-10T19:57:23Z
```

# Workspace versioning

---

_Pull request opened by @KGrewal1 on 2025-02-18 11:25_

Move "0.0.0" versions and "publish = false" to top level Cargo.toml and inherit from there - steps towards #43 (see https://github.com/astral-sh/ruff/issues/43#issuecomment-2628873363 - as means all the ruff internal packages can be versioned together)


---

_Review requested from @carljm by @KGrewal1 on 2025-02-18 11:25_

---

_Review requested from @MichaReiser by @KGrewal1 on 2025-02-18 11:25_

---

_Review requested from @AlexWaygood by @KGrewal1 on 2025-02-18 11:25_

---

_Review requested from @sharkdp by @KGrewal1 on 2025-02-18 11:25_

---

_Review requested from @BurntSushi by @KGrewal1 on 2025-02-18 11:25_

---

_Review requested from @dhruvmanila by @KGrewal1 on 2025-02-18 11:25_

---

_Comment by @github-actions[bot] on 2025-02-18 11:34_

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

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-02-18 11:34_

---

_Review comment by @MichaReiser on `crates/red_knot/Cargo.toml`:3 on 2025-02-18 12:35_

I don't think this works for us. Red Knot will use its own versioning schema

---

_@MichaReiser reviewed on 2025-02-18 12:36_

Thank you. I'm not sure if we want to use the same version for all crates. We definetely don't want to use the same version for Ruff and Red Knot crates. It's not clear to me how we would structure the workspace to make that work and publishing to crates.io currently isn't a priority to us.

That's why I'll close this for now.

---

_Closed by @MichaReiser on 2025-02-18 12:36_

---

_@KGrewal1 reviewed on 2025-02-18 12:48_

---

_Review comment by @KGrewal1 on `crates/red_knot/Cargo.toml`:3 on 2025-02-18 12:48_

Isn't this the current state of things for the foreseeable future tho? (Both ruff and red knot crates using the same versioning schema of everything at "0.0.0")

---

_@MichaReiser reviewed on 2025-02-18 12:51_

---

_Review comment by @MichaReiser on `crates/red_knot/Cargo.toml`:3 on 2025-02-18 12:51_

It's maybe for 1-2 more months but we then plan on releasing red knot with its own versioning schema.

---
