```yaml
number: 16758
title: "[red-knot] Add `--color` CLI option"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/color-cli-option2
created_at: 2025-03-14T18:49:08Z
updated_at: 2025-03-17T10:12:15Z
url: https://github.com/astral-sh/ruff/pull/16758
synced_at: 2026-01-12T15:55:56Z
```

# [red-knot] Add `--color` CLI option

---

_@MichaReiser_

## Summary

This PR adds a new `--color` CLI option that controls whether the output should be colorized or not. 

This is implements part of https://github.com/astral-sh/ty/issues/177 except that it doesn't implement the persistent configuration support as initially proposed in the CLI document. I realized, that having this as a persistent configuration is somewhat awkward because we may end up writing tracing logs **before** we loaded and resolved the settings. Arguably, it's probably fine to color the output up to that point, but it feels like a somewhat broken experience. That's why I decided not to add the persistent configuration option for now.


## Test Plan

I tested this change manually by running Red Knot with `--color=always`, `--color=never`, and `--color=auto` (or no argument) and verified that:

* The diagnostics are or aren't colored
* The tracing output is or isn't colored.


---

_Review requested from @carljm by @MichaReiser on 2025-03-14 18:49_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-03-14 18:49_

---

_Review requested from @sharkdp by @MichaReiser on 2025-03-14 18:49_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-14 18:50_

---

_Comment by @github-actions[bot] on 2025-03-14 18:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @sharkdp on `crates/red_knot/src/args.rs`:83 on 2025-03-14 19:40_

Maybe
```suggestion
    #[arg(long, value_name = "WHEN")]
```

---

_Review comment by @sharkdp on `crates/red_knot/src/args.rs`:258 on 2025-03-14 19:42_

This sounds a bit like we would actually detect if the terminal emulator supports colors. What's actually happening is just a `isatty` check (+ checks for `NO_COLOR` etc). So I tend to write something like
```suggestion
    /// Display colors if the output goes to an interactive terminal.
```

---

_Review comment by @sharkdp on `crates/red_knot/src/args.rs`:262 on 2025-03-14 19:43_

Minor: use a full stop here as well for consistency?
```suggestion
    /// Always display colors.
```

---

_@sharkdp approved on 2025-03-14 19:44_

Thank you!

---

_Comment by @sharkdp on 2025-03-14 20:02_

One more question: Would it make sense to move this option to the top level instead of `knot check`? It seems like other sub-commands might also make use of that option in the future. `uv` also has it as a top-level option.

---

_Comment by @MichaReiser on 2025-03-14 20:06_

> One more question: Would it make sense to move this option to the top level instead of `knot check`? It seems like other sub-commands might also make use of that option in the future. `uv` also has it as a top-level option.

Absolutely. There are options that we should move and I'll move them all together

---

_Merged by @MichaReiser on 2025-03-17 10:06_

---

_Closed by @MichaReiser on 2025-03-17 10:06_

---

_Branch deleted on 2025-03-17 10:06_

---

_Comment by @github-actions[bot] on 2025-03-17 10:12_

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
