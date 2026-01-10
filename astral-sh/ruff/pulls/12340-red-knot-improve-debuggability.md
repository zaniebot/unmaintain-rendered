```yaml
number: 12340
title: "[red-knot] Improve debuggability"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
base: main
head: db-dbg
created_at: 2024-07-15T18:00:52Z
updated_at: 2024-08-14T08:54:39Z
url: https://github.com/astral-sh/ruff/pull/12340
synced_at: 2026-01-10T21:38:31Z
```

# [red-knot] Improve debuggability

---

_Pull request opened by @AlexWaygood on 2024-07-15 18:00_

## Summary

This PR requires that all implementers of `ruff_db::db::Db` also implement `Debug`. The reason for doing this is it makes it much easier to debug variables that are typed as `&'db dyn Db`, and it means that we can simply `#[derive(Debug)]` on structs that have `db: &'db dyn Db` fields.

`Debug` needs to be manually implemented for all `TestDb` and `Program` classes, unfortunately, as `salsa::Storage` does not implement `Debug`, and all `TestDb` structs have a field of type `salsa::Storage`.


---

_Label `red-knot` added by @AlexWaygood on 2024-07-15 18:00_

---

_Review requested from @carljm by @AlexWaygood on 2024-07-15 18:00_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-15 18:00_

---

_Comment by @github-actions[bot] on 2024-07-15 18:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-07-15 18:19_

I'm not sure how useful a debug implementation on Db is that prints all its fields. Printing any Db will just result in a massive wall of text.

The main pain point seems to be that you can't derive debug on structs containing Db. If that's the real problem, maybe a wrapper struct that stores a reference to a Db with a simple Debug implementation that only prints <Db> is better suited for the task

---

_Comment by @AlexWaygood on 2024-07-16 10:23_

I've revised the PR so that the `Debug` implementations for the `TestDb` and `Program` structs only give a summary of what's in the database by default, but give a more verbose description of the contents if you use the alternate representation (`#?` rather than `?`).

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-16 10:23_

---

_Comment by @MichaReiser on 2024-07-16 11:15_

> I've revised the PR so that the Debug implementations for the TestDb and Program structs only give a summary of what's in the database by default, but give a more verbose description of the contents if you use the alternate representation (#? rather than ?).

I'm not sure if this distinction is that useful because `dbg` always uses the alternate representation. I still think a wrapper struct to enable the root cause, being able to automatically derive `Debug` is the better approach.

Taking one step back. What are the main use cases that we're trying to solve here? What are cases where you wanted to use `dbg` on a struct that can't implement `dbg` because of `db`?

---

_Review request for @carljm removed by @carljm on 2024-07-20 00:24_

---

_Comment by @MichaReiser on 2024-08-01 21:28_

I think my preferred solution here would be to start a discussion on the salsa zulip on whether `Storage` should implement a dummy `Debug`. It should then be trivial to derive `Debug` for all database implementations.

---

_Comment by @MichaReiser on 2024-08-14 08:49_

@AlexWaygood are you planning on following up on this PR or should we close it?

---

_Comment by @AlexWaygood on 2024-08-14 08:54_

It's not a priority so we can just close for now if you're looking to get the PR count down ;)

I can open a separate PR if I get to it

---

_Closed by @AlexWaygood on 2024-08-14 08:54_

---

_Branch deleted on 2024-08-14 08:54_

---
