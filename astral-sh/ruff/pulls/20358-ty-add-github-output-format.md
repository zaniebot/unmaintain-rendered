```yaml
number: 20358
title: "[ty] Add GitHub output format"
type: pull_request
state: merged
author: ntBre
labels:
  - cli
  - ty
assignees: []
merged: true
base: main
head: brent/ty-github
created_at: 2025-09-11T21:51:19Z
updated_at: 2025-09-17T13:50:27Z
url: https://github.com/astral-sh/ruff/pull/20358
synced_at: 2026-01-12T15:57:00Z
```

# [ty] Add GitHub output format

---

_@ntBre_

## Summary

This PR wires up the GitHub output format moved to `ruff_db` in #20320 to the ty CLI.

It's a bit smaller than the GitLab version (#20155) because some of the helpers were already in place, but I did factor out a few `DisplayDiagnosticConfig` constructor calls in Ruff. I also exposed the `GithubRenderer` and a wrapper `DisplayGithubDiagnostics` type because we needed a way to configure the program name displayed in the GitHub diagnostics. This was previously hard-coded to `Ruff`:

<img width="675" height="247" alt="image" src="https://github.com/user-attachments/assets/592da860-d2f5-4abd-bc5a-66071d742509" />

Another option would be to drop the program name in the output format, but I think it can be helpful in workflows with multiple programs emitting annotations (such as Ruff and ty!)

## Test Plan

New CLI test, and a manual test with `--config 'terminal.output-format = "github"'`


---

_Label `cli` added by @ntBre on 2025-09-11 21:51_

---

_Label `ty` added by @ntBre on 2025-09-11 21:51_

---

_Comment by @github-actions[bot] on 2025-09-11 21:53_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-11 21:56_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-09-11 22:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-09-11 23:52_

---

_Review requested from @carljm by @ntBre on 2025-09-11 23:52_

---

_Review requested from @MichaReiser by @ntBre on 2025-09-11 23:52_

---

_Review requested from @sharkdp by @ntBre on 2025-09-11 23:52_

---

_Review requested from @dcreager by @ntBre on 2025-09-11 23:52_

---

_Review comment by @dhruvmanila on `crates/ruff_db/src/diagnostic/render/snapshots/ruff_db__diagnostic__render__github__tests__output.snap`:7 on 2025-09-13 10:15_

Just confirming that this change is basically just a side effect of having ty as the default for `Program` and is not because there's someplace where the program is not being set correctly?

---

_Review comment by @dhruvmanila on `crates/ruff_db/src/diagnostic/mod.rs`:1404 on 2025-09-13 10:19_

I think at this point, it might be useful to refactor this into a `new` method that takes in a `program: Program` as the sole parameter because there's no valid default value for this field and the caller should always provide the program for which this rendering is to be done for. What do you think?

---

_Review comment by @dhruvmanila on `crates/ruff_db/src/diagnostic/mod.rs`:1317 on 2025-09-13 10:19_

Refer to my other comment, I'd probably remove the default value for this field.

---

_@dhruvmanila approved on 2025-09-13 10:20_

This looks great!

I just have one minor concern, refer to inline comment.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:1473 on 2025-09-15 11:41_

Nit: It would probably be fine to just use a `String` here

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:1318 on 2025-09-15 11:45_

I think I brought this up before and it isn't something that we need to change in this PR but I personally find it a bit difficult to reason about which settings are used for which output format (most settings are only used by one or very few formats). 

The main reason I see today why we need a single `Config` struct is because `DisplayDiagnostics` matches on the output format, which I'm not sure we should keep because we also match over the output format in ruff's rendering code (meaning, we first match to create the generic configuration only to then match on the format again to call into specific renderers). To me, it would feel simpler to just have a single match in ruff's printer that calls the corresponding renderer directly

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/github.rs`:21 on 2025-09-15 11:45_

I think we want to use the correct severity for ty diagnostics and not always default to error

---

_@MichaReiser approved on 2025-09-15 11:46_

---

_@ntBre reviewed on 2025-09-15 13:35_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:1318 on 2025-09-15 13:35_

I thought eventually we would be able to hoist the `Config` out of the match in Ruff and have something like `DisplayDiagnosticsConfig::from(ruff_output_format)` and a single `DisplayDiagnostics` call, but I guess you're right. We may not want to port over all of the output formats, and some other logic is mixed in on the Ruff side, such as the `SHOW_FIX_SUMMARY` flag and writing of summary text. I can follow up on this.

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/snapshots/ruff_db__diagnostic__render__github__tests__output.snap`:7 on 2025-09-15 13:48_

Yes, that's right!

---

_@ntBre reviewed on 2025-09-15 13:48_

---

_Comment by @ntBre on 2025-09-15 13:56_

Thanks for the reviews! I pushed one commit to use the real severity. I think Dhruv's suggestions about not having a default `Program` make a lot of sense, but it looks like the `Program` config field can go away entirely with Micha's restructuring suggestion (we can just pass a `&str` to `GithubRenderer::new` instead of a config). 

I briefly started that refactor on this branch, but I think it would help to finally make the `DisplayDiagnostics` rendering take a `std::io::Write` instead of a `std::fmt::Write`, so it seems best to address these all at once in an immediate follow-up PR.

---

_Merged by @ntBre on 2025-09-17 13:50_

---

_Closed by @ntBre on 2025-09-17 13:50_

---

_Branch deleted on 2025-09-17 13:50_

---
