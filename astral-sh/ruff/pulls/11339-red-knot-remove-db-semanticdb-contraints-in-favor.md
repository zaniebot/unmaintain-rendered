```yaml
number: 11339
title: "[red-knot] Remove `<Db: SemanticDb>` contraints in favor of dynamic dispatch"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: db-with-jar
created_at: 2024-05-08T14:38:55Z
updated_at: 2024-05-08T16:07:15Z
url: https://github.com/astral-sh/ruff/pull/11339
synced_at: 2026-01-12T15:55:37Z
```

# [red-knot] Remove `<Db: SemanticDb>` contraints in favor of dynamic dispatch

---

_@MichaReiser_

## Summary

We used to have a mixture of functions/methods that accept `&dyn Db + HasJar` and methods that were generic over `Db`. 

The problem with this approach is that calling a method that accepted a `Db: HasJar` wasn't possible when only holding a reference to a `&dyn Db` or it required repeating the `HasJar` constraint. 

This PR aligns our traits with Salsa by:

* Introducing a new `DbWithJar<Jar>` trait. The trait itself is empty, but it requires the type to implement `HasJar<Jar>` and `Database` 
* Make `DbWithJar` a base trait for all the `Db` traits (`SourceDb`, `SemanticDb`, `LintDb` etc)
* Change all methods to take `db: &dyn SemanticDb` as the argument. This simplifies the function signatures a lot 


The last change isn't strictly related and maybe I should have done it as a separate PR. However, it removes the 
query and mutation methods from the database traits because we can just call the functions directly. This aligns our API with salsa20202. 

I've mixed feelings about it. It is nice, because it reduces repetition. However, it does reduce discoverability because you can no longer write
`db.` to discover all queries/mutations. 

## Test Plan

`cargo build`


---

_Label `internal` added by @MichaReiser on 2024-05-08 14:39_

---

_Review comment by @MichaReiser on `crates/red_knot/src/program/mod.rs`:79 on 2024-05-08 14:41_

Creating a Db is now a bit more work but I don't mind that. We should have at most one Db per crate.

---

_@MichaReiser reviewed on 2024-05-08 14:41_

---

_Comment by @github-actions[bot] on 2024-05-08 14:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @carljm by @MichaReiser on 2024-05-08 16:03_

---

_@carljm approved on 2024-05-08 16:04_

Looks good!

---

_Merged by @MichaReiser on 2024-05-08 16:07_

---

_Closed by @MichaReiser on 2024-05-08 16:07_

---

_Branch deleted on 2024-05-08 16:07_

---
