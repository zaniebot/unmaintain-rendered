```yaml
number: 18498
title: "[ty] File inclusion and exclusion"
type: pull_request
state: merged
author: MichaReiser
labels:
  - configuration
  - ty
assignees: []
merged: true
base: main
head: micha/include-exclude
created_at: 2025-06-06T14:20:29Z
updated_at: 2025-06-12T17:08:13Z
url: https://github.com/astral-sh/ruff/pull/18498
synced_at: 2026-01-10T18:45:04Z
```

# [ty] File inclusion and exclusion

---

_Pull request opened by @MichaReiser on 2025-06-06 14:20_

## Summary

This PR adds two new configuration options:

* `src.include`: gitignore-like glob patterns but inverted: `src` includes the directory `src` instead of excluding it. This is similar to npm's [`files`](https://docs.npmjs.com/cli/v11/configuring-npm/package-json#files) option in the `package.json`.
* `src.exclude`: gitignore-like glob patterns that exclude files from analysis. Exclude takes precedence over include. 

This PR also adds `--exclude` to the CLI. It allows adding more exclude patterns. I decided against adding an `--include` option because it is redundant to `ty check <paths>` which already allows to include additional files. The only difference between `ty check <paths>` is that paths specified like that take precedence over `exclude`. We could consider adding an `--include` option as an alternative to Ruff's `--force-exclude`. 

Sorry, this PR got a bit large but I required everything in place to verify that the ergonomics are what we want. 


Closes https://github.com/astral-sh/ty/issues/176

## Test Plan

Added CLI and unit tests

---

_Label `configuration` added by @MichaReiser on 2025-06-06 14:20_

---

_Label `ty` added by @MichaReiser on 2025-06-06 14:20_

---

_Label `configuration` added by @MichaReiser on 2025-06-06 14:20_

---

_Label `ty` added by @MichaReiser on 2025-06-06 14:20_

---

_Review comment by @MichaReiser on `crates/ty_project/src/glob/include.rs`:1 on 2025-06-06 14:21_

Most of the credit for this goes to @konstin because I shamelessly copied most from the uv implementation.

---

_@MichaReiser reviewed on 2025-06-06 14:21_

---

_Comment by @github-actions[bot] on 2025-06-06 14:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @MichaReiser on 2025-06-06 14:26_

Ugh, Windows isn't happy with at least one of my hacks :(

---

_Comment by @github-actions[bot] on 2025-06-06 16:44_

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

_Marked ready for review by @MichaReiser on 2025-06-09 07:08_

---

_Review requested from @carljm by @MichaReiser on 2025-06-09 07:08_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-09 07:08_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-09 07:08_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-09 07:08_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-06-09 18:30_

---

_Review request for @carljm removed by @carljm on 2025-06-10 17:06_

---

_Review comment by @BurntSushi on `crates/ty/docs/configuration.md`:165 on 2025-06-12 12:18_

I think I would be tempted to word this as, "`**` matches zero or more path components." So like, `**a` is kind of a weird glob. I think `globset` does currently accept that and does implement the semantics you describe here though.

---

_Review comment by @BurntSushi on `crates/ty/docs/configuration.md`:168 on 2025-06-12 12:18_

What about `{a,b,...}`?

---

_Review comment by @BurntSushi on `crates/ty/tests/cli/file_selection.rs`:621 on 2025-06-12 12:20_

Ah I see you're way ahead of me about `**a` haha.

---

_Review comment by @BurntSushi on `crates/ty/docs/configuration.md`:168 on 2025-06-12 12:28_

Oh nevermind, you're using your own portable glob syntax that doesn't have this. Makes sense.

---

_@BurntSushi approved on 2025-06-12 12:29_

Nice! Reviewing this ended up being pretty easy because we've already talked so much about the semantics. :-)

---

_Merged by @MichaReiser on 2025-06-12 17:07_

---

_Closed by @MichaReiser on 2025-06-12 17:07_

---

_Branch deleted on 2025-06-12 17:07_

---
