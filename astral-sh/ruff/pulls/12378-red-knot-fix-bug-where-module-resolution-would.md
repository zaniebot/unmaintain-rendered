```yaml
number: 12378
title: "[red-knot] Fix bug where module resolution would not be invalidated if an entire package was deleted"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: resolver-salsa-invalidation
created_at: 2024-07-18T13:00:58Z
updated_at: 2024-07-19T12:56:41Z
url: https://github.com/astral-sh/ruff/pull/12378
synced_at: 2026-01-12T15:55:41Z
```

# [red-knot] Fix bug where module resolution would not be invalidated if an entire package was deleted

---

_@AlexWaygood_

## Summary

This fixes a bug where module resolution would not be invalidated if an entire package was deleted

## Test Plan

I added a test that fails on `main`, and passes with this PR.


---

_Label `red-knot` added by @AlexWaygood on 2024-07-18 13:00_

---

_Review requested from @carljm by @AlexWaygood on 2024-07-18 13:00_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-18 13:00_

---

_Comment by @github-actions[bot] on 2024-07-18 13:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:390 on 2024-07-18 13:51_

Nit: We can use `vendored().exists` here because we know that the system is read only.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:1621 on 2024-07-18 13:52_

Do we have to delete these files as well? If so, should we call `touch` for them?

---

_@MichaReiser approved on 2024-07-18 13:52_

---

_@AlexWaygood reviewed on 2024-07-18 13:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:390 on 2024-07-18 13:54_

I wondered about this. In what situation _should_ we use `vendored_path_to_file()`?

---

_@MichaReiser reviewed on 2024-07-18 13:57_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:390 on 2024-07-18 13:57_

In cases where we need a `File`, for example because we have to pass it to `source_text`

---

_@AlexWaygood reviewed on 2024-07-18 13:59_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:390 on 2024-07-18 13:59_

Right -- but if we know it's immutable, maybe we should just be calling `vendored.read-to_string()` instead of `source_text()` to get the source text -- the same argument applies, no? (Not trying to be argumentative, just curious!)

---

_@MichaReiser reviewed on 2024-07-18 14:08_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:390 on 2024-07-18 14:08_

It depends. Not if it is a Python file, because calling `source_text` then has the benefit that we read the vendored python file exactly once instead of multiple times (`file.read_to_string` is not cached). 

That's also not the point I made above. We need `vendored_path_to_file` when calling `parsed_module` because the function takes a `File` argument (because it doesn't care if it is a vendored or non vendored file). That's when you need a file. 

---

_@AlexWaygood reviewed on 2024-07-18 14:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:390 on 2024-07-18 14:39_

> `file.read_to_string` is not cached

Nor is `vendored.exists()` -- each time we call that, we'll be querying the zip archive. Is the logic that we think that this will be inexpensive enough that it's not worth caching?

---

_@AlexWaygood reviewed on 2024-07-18 15:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:1621 on 2024-07-18 15:16_

Argh, you're right. This test doesn't exercise anything at all right now :( It passes on `main` if I `touch` both files.

---

_@MichaReiser reviewed on 2024-07-18 16:03_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:390 on 2024-07-18 16:03_

I'm not sure I understand. This seems to be unrelated to the original question.

>  it's not worth caching

Probably not. Caching comes at a high cost (memory, locking, lookup). We cache the resolved module, which should prevent from calling into the vendored system in the first place. But we have to wait for some real world usage to tell.

---

_@AlexWaygood reviewed on 2024-07-18 16:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:390 on 2024-07-18 16:23_

> This seems to be unrelated to the original question.

It seems related to me, but perhaps this illustates confusion on my part :-) 

I'm trying to understand when and why (in future PRs) I should go via the `Files` APIs for reading information about files stored in the vendored zip archive, and when it makes sense to just get this information from the `VendoredFileSystem`. If I understand correctly, the tradeoffs are as follows:
1. Going via the `Files` API has the advantage that the query is automatically invalidated when the file changes -- but the file will never change for vendored files, so, unlike with filesystem files, that's not a good reason to go via the `Files` API.
2. Going via the `Files` API has the advantage that the call is automatically cached. This means that if we go via the `Files` API, we'll end up querying the zip archive less frequently; but that could also cost more than it saves us, due to higher memory allocation/consumption, and because locking and lookups in the cache are also costly.
3. Going via the `Files` API gives you a `File` object that you can pass directly to queries such as `ruff_db::parsed::parsed_module`. You wouldn't be able to call these queries if you just read the contents of the file as a string using the `VendoredFileSystem` directly.

Is that an accurate summary? I feel like my question has been answered now, anyway!

---

_@MichaReiser reviewed on 2024-07-18 16:33_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:390 on 2024-07-18 16:33_

I think that's a good summary. I now see how it is related. 

Regarding `vendored_path_to_file(...).exists`. This is actually not cached for files that don't exist. We could, but we currently don't to avoid tracking `File`s for vendored paths that don't exist. So `vendored().exists` and `vendored_path_to_file(path).exists` have the same cost for files that don't exist.

---

_@AlexWaygood reviewed on 2024-07-19 12:51_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:1621 on 2024-07-19 12:51_

I removed the test, since your file-watching PR adds tests that fail reliably without this fix

---

_Merged by @AlexWaygood on 2024-07-19 12:53_

---

_Closed by @AlexWaygood on 2024-07-19 12:53_

---

_Branch deleted on 2024-07-19 12:53_

---
