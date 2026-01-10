```yaml
number: 11837
title: "red-knot[salsa part 2]: Setup semantic DB and Jar"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: salsa-2-semantic-jar
created_at: 2024-06-11T14:07:05Z
updated_at: 2024-06-13T07:13:11Z
url: https://github.com/astral-sh/ruff/pull/11837
synced_at: 2026-01-10T21:56:00Z
```

# red-knot[salsa part 2]: Setup semantic DB and Jar

---

_Pull request opened by @MichaReiser on 2024-06-11 14:07_

## Summary

This PR sets up an empty Salsa `Jar` and `Db`. This is mostly boring template code but I thought it's worth separating it form an actual interesting PR ;)

## Test Plan

`cargo build`


---

_Label `red-knot` added by @MichaReiser on 2024-06-11 14:07_

---

_Review requested from @carljm by @MichaReiser on 2024-06-11 14:07_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-06-11 14:07_

---

_Comment by @github-actions[bot] on 2024-06-11 14:27_

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

_Review comment by @carljm on `crates/ruff_db/src/source.rs`:80 on 2024-06-12 13:33_

It's surprising to me that we have to manually update the revision here; should `write_file` do that itself?

---

_@carljm approved on 2024-06-12 13:38_

I still have some concerns about having this code just mixed in directly with the rest of the modules in `ruff_python_semantic` and using only features to distinguish what is red-knot, but otherwise this PR looks fine, and I assume we will revisit that structure as follow-up, not on this PR, for rebase-conflict reasons.

---

_@MichaReiser reviewed on 2024-06-12 13:47_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/source.rs`:80 on 2024-06-12 13:47_

The `FileSystem` that I have in mind should be independent of the `vfs` and salsa db. It's just an abstraction over `std::fs`. What I have in mind to avoid the `set_revision` call here is that we have an `apply_changes` method where we pass all file changes (adds, remove, modified) that updates the vfs state. It's probably the `FileSystem'`s or the host's responsibility to collect all made changes. 

---

_@carljm reviewed on 2024-06-12 14:00_

---

_Review comment by @carljm on `crates/ruff_db/src/source.rs`:80 on 2024-06-12 14:00_

That makes sense, and would address my concern: I don't want to expose/use APIs that make it easy to change a file's contents but forget to bump its revision.

I think this also gets back to what I find confusing about the `Vfs` and `FileSystem` structure, which is that there is no single entity which really encapsulates our virtual file system. Where does this `apply_changes` method live? I guess it is just a free function that takes a `Db` and some changes to apply?

Probably this is just something I have to get used to with Salsa, that the `Db` owns all the state, so most API that we use for managing state is just a function that takes a `Db`.

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/lib.rs`:85 on 2024-06-12 16:43_

It wasn't immediately obvious to me here that "take" was a referende to `std::mem::take`; I'd find this slightly clearer:

```suggestion
        /// Empties the internal store of salsa events that have been emitted,
        /// and returns them as a `Vec` (equivalent to [`std::mem::take`]).
```

---

_@AlexWaygood approved on 2024-06-12 16:44_

---

_@MichaReiser reviewed on 2024-06-13 06:56_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/source.rs`:80 on 2024-06-13 06:56_

> Where does this apply_changes method live? I guess it is just a free function that takes a Db and some changes to apply?

It would probably be a free standing function that takes `&mut Db`. And yes, the fact that it is a free standing function takes some time to get used to. An alternative is to make it a method in `Vfs`, but I think that won't work because it would require holding a mutable reference to `Db` and a read only reference to `FileSystem`. 

> Probably this is just something I have to get used to with Salsa, that the Db owns all the state, so most API that we use for managing state is just a function that takes a Db.

yes, that takes some time to get used to. It's also something I'm still figuring out 

---

_Comment by @MichaReiser on 2024-06-13 06:57_

> I still have some concerns about having this code just mixed in directly with the rest of the modules in ruff_python_semantic and using only features to distinguish what is red-knot, but otherwise this PR looks fine, and I assume we will revisit that structure as follow-up, not on this PR, for rebase-conflict reasons.

I think it's easiest if we discuss this in person in our planning meeting on Friday.

---

_Merged by @MichaReiser on 2024-06-13 07:00_

---

_Closed by @MichaReiser on 2024-06-13 07:00_

---

_Branch deleted on 2024-06-13 07:00_

---
