```yaml
number: 17686
title: "[red-knot] ruff_db: render file paths in diagnostics as relative paths if possible"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: ag/diagnostic-relative-path
created_at: 2025-04-28T17:05:32Z
updated_at: 2025-04-28T18:32:36Z
url: https://github.com/astral-sh/ruff/pull/17686
synced_at: 2026-01-10T19:03:00Z
```

# [red-knot] ruff_db: render file paths in diagnostics as relative paths if possible

---

_Pull request opened by @BurntSushi on 2025-04-28 17:05_

This is done in what appears to be the same way as Ruff: we get the CWD,
strip the prefix from the path if possible, and use that. If stripping
the prefix fails, then we print the full path as-is.

Fixes #17233


---

_Review requested from @carljm by @BurntSushi on 2025-04-28 17:05_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-04-28 17:05_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-04-28 17:05_

---

_Review requested from @sharkdp by @BurntSushi on 2025-04-28 17:05_

---

_Review requested from @dcreager by @BurntSushi on 2025-04-28 17:05_

---

_Renamed from "ruff_db: render file paths in diagnostics as relative paths if possible" to "[red-knot] ruff_db: render file paths in diagnostics as relative paths if possible" by @BurntSushi on 2025-04-28 17:06_

---

_Label `red-knot` added by @BurntSushi on 2025-04-28 17:06_

---

_Label `diagnostics` added by @BurntSushi on 2025-04-28 17:06_

---

_Review request for @dcreager removed by @BurntSushi on 2025-04-28 17:06_

---

_Review request for @carljm removed by @BurntSushi on 2025-04-28 17:06_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-04-28 17:06_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:701 on 2025-04-28 17:09_

We should use `system.current_directory` here (which is also cheap). That should also remove the need to do any wasm feature gating.

---

_@MichaReiser approved on 2025-04-28 17:10_

Nice! 

We aren't allowed to use any system specific APIs in red knot outside of the CLI crate (because we know that it's always OS) without going through the `System` crate or we break our platform abstraction.

---

_Comment by @github-actions[bot] on 2025-04-28 17:15_

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

_@BurntSushi reviewed on 2025-04-28 17:23_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:701 on 2025-04-28 17:23_

Oooo nice! I did look around for something like this briefly, but missed it.

The change to this did cause a huge number of snapshot changes. But it seems to just be limited to removing the leading `/` from file paths. Which seems like a positive change. (I guess `get_cwd()` was returning the path without a trailing `/`, but `system.current_directory()` includes a trailing `/`.)

---

_@MichaReiser reviewed on 2025-04-28 17:36_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:701 on 2025-04-28 17:36_

I think the reason is a different one. Mdtests run with cwd set to the Ruff workspace root (I think) which isn't a prefix of `/src`. However, `System` is initialized to `/` (we could think about changing it to `/src` actually) which is a prefix of `/src`

---

_@MichaReiser reviewed on 2025-04-28 17:41_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:701 on 2025-04-28 17:41_

That makes me wonder. Should we add a CLI test that shows how diagnostics are rendered if a file is in a parent directory?

---

_@BurntSushi reviewed on 2025-04-28 17:49_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:701 on 2025-04-28 17:49_

Aye, done. It behaves as I would expect given the implementation: it behaves like it used to and prints the full absolute path.

Probably this case can be improved too, but it strikes me as lower priority.

---

_@MichaReiser approved on 2025-04-28 17:55_

---

_@MichaReiser reviewed on 2025-04-28 18:00_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:611 on 2025-04-28 18:00_

Sorry for my piece by piece review. 

I assume that doing this here ensures that this change applies to both the full and concise rendering?

I'm asking because that would explain why mypy primer hangs (too large diffs :D)

---

_@BurntSushi reviewed on 2025-04-28 18:32_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:611 on 2025-04-28 18:32_

Yes. There are snapshot tests that do cover this.

Good to know about mypy primer hanging. Makes sense, since I think just about ~every diagnostic is impacted by this change.

---

_Comment by @BurntSushi on 2025-04-28 18:32_

Merging despite mypy primer hang. See: https://github.com/astral-sh/ruff/pull/17686#discussion_r2064265801

---

_Merged by @BurntSushi on 2025-04-28 18:32_

---

_Closed by @BurntSushi on 2025-04-28 18:32_

---

_Branch deleted on 2025-04-28 18:32_

---
