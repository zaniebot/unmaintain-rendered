```yaml
number: 12257
title: "[red-knot] Rename some methods on `VendoredFileSystem` for consistency"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
base: main
head: vendored-naming-consistency
created_at: 2024-07-09T09:26:00Z
updated_at: 2024-07-09T17:18:02Z
url: https://github.com/astral-sh/ruff/pull/12257
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] Rename some methods on `VendoredFileSystem` for consistency

---

_Pull request opened by @AlexWaygood on 2024-07-09 09:26_

## Summary

Following https://github.com/astral-sh/ruff/commit/ac04380f360b1fca84435fbbf3154f61e551a871, we have a situation where the `ruff_db::system::System` trait has methods such as `path_exists()` and `path_metadata`, but the `VendoredFileSystem` struct simply calls its equivalent methods `exists()` and `metadata`. This leads to some funny-looking code here, where the two branches are doing equivalent operations with different types, but the methods being called have two different names:

https://github.com/astral-sh/ruff/blob/6fa4e32ad318afbec4c6997c69731bb4c256ef82/crates/red_knot_module_resolver/src/path.rs#L330-L333

This PR renames the methods on `VendoredFileSystem` to be consistent with the new names for the methods on the `System` trait.


---

_Label `red-knot` added by @AlexWaygood on 2024-07-09 09:26_

---

_Review requested from @carljm by @AlexWaygood on 2024-07-09 09:26_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-09 09:26_

---

_Comment by @github-actions[bot] on 2024-07-09 09:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-07-09 10:25_

I prefer the old names because `vendored` is a file system only and using the longer names aren't needed to disambiguate. It only is strange here because the two interfaces are used alongside each other. If they're used separately, the old names are more brief.

---

_Comment by @AlexWaygood on 2024-07-09 10:48_

Yeah. I don't feel strongly. Let's see what @carljm thinks.

---

_Comment by @carljm on 2024-07-09 17:11_

I also don't feel strongly here and would be fine either way, but if I have to choose a preference I agree with @MichaReiser: `VendoredFileSystem` is a filesystem, so the name `exists()` is clear as-is; `System` covers more than just the filesystem, so the name needs disambiguation. `System` and `VendoredFileSystem` are not parallel, so it's ok if their method names aren't parallel.

---

_Closed by @AlexWaygood on 2024-07-09 17:18_

---

_Branch deleted on 2024-07-09 17:18_

---
