```yaml
number: 15645
title: "Add `rules` table to configuration"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: micha/rule-table
created_at: 2025-01-21T15:34:16Z
updated_at: 2025-01-23T09:57:01Z
url: https://github.com/astral-sh/ruff/pull/15645
synced_at: 2026-01-12T15:55:52Z
```

# Add `rules` table to configuration

---

_@MichaReiser_

## Summary

This PR adds support for enabling or disabling a rule or change its severity to warning by using a configuration file. 

```toml
[tool.knot.rules]
division-by-zero = "error"
```

This PR doesn't yet add support for the `rule = { severity="level" }` format simply because we don't support any per-rule configurations other than `severity` just yet (like fixability). We should add support once for a sub-table once we add support for it. This PR also does not yet add support for configuring rules over the CLI.

The "hardest" part of this PR was to make the diagnostics propagate. The approach
I've taken for now is to simply collect them when calling `project.check` or `project.check_file`. 
I don't love this approach because it's somewhat easy to get wrong and we have to repeat it in 
every entry function that may return diagnostics (e.g. `format`?). However, 
we currently only exactly have two, so it sort of feels okay? 

I considered using salsa accumulators but doing so wouldn't solve 
the problem that we have to remember collecting the diagnostics  
in the `project.check` call, but we could use them if we want, 
performance isn't as much of a concern here. 

Overall I felt like defering the ideal design until we have
more settings that require validation to get a better sense of
how awkward (or good) the current design is. 

The diagnostics for unknown rules currently lack any source information. I plan on adding proper spans in a follow-up PR because it requires some machinery to funnel the spans through serde. 

CC @BurntSushi: I don't consider the "how we funnel diagnostics through red knot" as the main focus of your diagnostics work but I thought this might still be interesting to you. 

Part of https://github.com/astral-sh/ty/issues/219

## Test Plan

Added CLI tests. 

<!-- How was it tested? -->


---

_Label `red-knot` added by @MichaReiser on 2025-01-21 15:34_

---

_Label `diagnostics` added by @MichaReiser on 2025-01-21 15:34_

---

_@MichaReiser reviewed on 2025-01-21 15:36_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/lib.rs`:153 on 2025-01-21 15:36_

I included the rule diagnostics in `check_file` because an API consumer (e.g., the LSP) can decide only to call `check_file` but never `project.check` and would never see the diagnostics. 

The counter-argument is that these diagnostics aren't related to `file` so they shouldn't be shown. 

I'm not sure what the right answer is, but I defaulted to including them for now. 

---

_Marked ready for review by @MichaReiser on 2025-01-21 15:40_

---

_Review requested from @carljm by @MichaReiser on 2025-01-21 15:40_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-21 15:40_

---

_Review requested from @sharkdp by @MichaReiser on 2025-01-21 15:40_

---

_Comment by @github-actions[bot] on 2025-01-21 15:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot/tests/cli.rs`:201 on 2025-01-22 20:56_

Warnings are not shown by default?

---

_Review comment by @carljm on `crates/red_knot/tests/cli.rs`:225 on 2025-01-22 20:57_

Hmm, I'm surprised to see that we currently make `unresolved-reference` a warning by default, I would think it should be high confidence (and it is high severity, since your program will just crash).

---

_Review comment by @carljm on `crates/red_knot/tests/cli.rs`:290 on 2025-01-22 20:58_

I suppose including a pointer here to the config file is for a future PR/improvement?

---

_@carljm approved on 2025-01-22 21:29_

Sweet!

---

_@MichaReiser reviewed on 2025-01-22 21:46_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/cli.rs`:290 on 2025-01-22 21:46_

Yes

---

_@MichaReiser reviewed on 2025-01-22 21:47_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/cli.rs`:201 on 2025-01-22 21:47_

They are always shown, but knot wont exit successfully (may not be implemented yed)

---

_@MichaReiser reviewed on 2025-01-22 21:49_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/cli.rs`:225 on 2025-01-22 21:49_

Hmm let me double check this test tomorrow. This should be possibly-unresolved-reference

---

_Review comment by @carljm on `crates/red_knot/tests/cli.rs`:201 on 2025-01-22 22:00_

Then I would expect a division-by-zero warning to be in the output of this test, but it isn't there?

Oh, I think it isn't there because you have `x / 0` where `x` is unbound, and we only emit division-by-zero if the dividend is a literal integer type (otherwise it might have redefined `__div__` such that dividing by zero is fine).

This seems pretty confusing, I wouldn't even involve `division-by-zero` rule in this test at all, if it doesn't have a case where we would actually emit that diagnostic.

---

_Review comment by @carljm on `crates/red_knot/tests/cli.rs`:225 on 2025-01-22 22:01_

I don't think so, this is from `y = x / 0`, where `x` is definitely-unbound.

`possibly-unresolved-reference` is marked as `ignore` in the test config, so is not shown (even though it occurs later in the `print(x)` line).

To be clear, this comment isn't suggesting that anything is wrong with the test, just that I'm surprised we make `unresolved-reference` a warning by default. (Which we clearly do, in diagnostics.rs).

---

_@carljm reviewed on 2025-01-22 22:03_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/cli.rs`:201 on 2025-01-23 09:07_

I do want to see a `division-by-zero` error to ensure that the demoting from `error` to warn works. So I have to change the test 

---

_@MichaReiser reviewed on 2025-01-23 09:07_

---

_Merged by @MichaReiser on 2025-01-23 09:56_

---

_Closed by @MichaReiser on 2025-01-23 09:56_

---

_Branch deleted on 2025-01-23 09:57_

---
