```yaml
number: 11215
title: "[red-knot] Integrate formatter"
type: pull_request
state: closed
author: MichaReiser
labels:
  - internal
assignees: []
base: main
head: red-knot-format
created_at: 2024-04-30T14:58:46Z
updated_at: 2024-07-22T07:44:55Z
url: https://github.com/astral-sh/ruff/pull/11215
synced_at: 2026-01-12T15:55:37Z
```

# [red-knot] Integrate formatter

---

_@MichaReiser_

## Summary

This is a first hacky approach to integrate the formatter into red-knot. 

There are plenty of TODOs. The main questions is where and when we should write the formatted content and how we avoid that formatting requires two-passes for the cache to be "hot". 

## Test Plan

I ran red knot and it formated my files (all lints gone, whoops)


---

_Label `internal` added by @MichaReiser on 2024-04-30 15:00_

---

_Converted to draft by @MichaReiser on 2024-04-30 15:15_

---

_Comment by @github-actions[bot] on 2024-04-30 15:17_

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

_@MichaReiser reviewed on 2024-04-30 16:04_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:164 on 2024-04-30 16:04_

That's a bit hacked in. We can also decide to remove it for now. But it does show that formatting after linting is possible (but requires a `&mut Program`)

---

_Marked ready for review by @MichaReiser on 2024-04-30 16:06_

---

_Review requested from @carljm by @MichaReiser on 2024-05-01 07:11_

---

_Review comment by @carljm on `crates/red_knot/src/format.rs`:74 on 2024-05-01 17:14_

for consistency?

```suggestion
pub(crate) fn check_file_formatted<Db>(db: &Db, file_id: FileId) -> Result<Diagnostics, FormatError>
```

---

_Review comment by @carljm on `crates/red_knot/src/program/format.rs`:16 on 2024-05-01 17:26_

It seems like mostly this is a good thing? The file content has changed, the rest of the system should react accordingly, as it normally would. I don't think we should special-case re-formatting in the rest of our invalidation -- it's a file change like any other.

The only place where this seems undesirable is that you would like to track that a file that we just formatted is still formatted; we don't have to reformat it. So I would scope any special-casing to this specific point only, contained within the formatter logic. Maybe we could keep a cache mapping the hash of known-well-formatted contents for a file, so when we get updated sources for a file from the file watcher, we can check if the hash of the new contents matches an entry in this cache, and if so, directly mark it as already well-formatted?

---

_Review comment by @carljm on `crates/red_knot/src/program/mod.rs`:149 on 2024-05-01 17:26_

```suggestion
        check_file_formatted(self, file_id)
```

---

_@carljm approved on 2024-05-01 17:27_

---

_Comment by @MichaReiser on 2024-05-10 06:48_

I still plan to land this PR eventually but aren't prioritising it right now. Deciding on a query system and exploring persistent caching is of higher importance right now and the prototype itself was enough for me to learn a lot about the challenge when it comes to integrate the formatter.

---

_Closed by @MichaReiser on 2024-07-22 07:44_

---
