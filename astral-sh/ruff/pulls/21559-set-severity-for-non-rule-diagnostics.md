```yaml
number: 21559
title: Set severity for non-rule diagnostics
type: pull_request
state: merged
author: senekor
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: senekor/lmkxkqrssyst
created_at: 2025-11-21T13:57:54Z
updated_at: 2025-11-21T17:56:13Z
url: https://github.com/astral-sh/ruff/pull/21559
synced_at: 2026-01-10T16:48:02Z
```

# Set severity for non-rule diagnostics

---

_Pull request opened by @senekor on 2025-11-21 13:57_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This change makes ruff server emit LSP diagnostics with severity level in more situations. Specifically, if the diagnostic didn't have a "secondary code" (I'm not super familiar what that is.) ruff server would previously emit _no_ severity.

This isn't good, because we always have an internal severity, so the information is right there. Editor experience is degraded without the severity information. For example, I use Helix. Themes typically don't consider the case of diagnostics without severity, and the fallback style is very ugly on most themes. Another functional problem is that Helix has a "diagnostics picker", which can filter by severity. This allows users to focus on errors before warnings. It doesn't work without the severity information.

I also added `tags(diagnostic)` to the same code path, because the information also doesn't seem to depend on the "secondary code". However, I haven't noticed this information missing in my own use of ruff, so I didn't confirm the difference in my own testing.

## Test Plan

I saw diagnostics without severity in my editor (Helix), made the fix, installed ruff, restarted LSP in Helix, confirmed that diagnostics now have severity.

A very simple file to confirm this just contains `if`, which will emit a "Expected an expression" diagnostic. With this PR, it has the severity "error", without it it has none.

---

_@MichaReiser reviewed on 2025-11-21 14:01_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/lint.rs`:294 on 2025-11-21 14:01_

Thank you. Hmm, this seems like an oversight to me from our diagnostic refactor. @ntBre could we always use `diagnostic.severity` here?

---

_@ntBre reviewed on 2025-11-21 14:28_

---

_Review comment by @ntBre on `crates/ruff_server/src/lint.rs`:294 on 2025-11-21 14:28_

Yeah it looks like it to me, it does seem like an oversight because I couldn't use the `severity` helper.

---

_@MichaReiser reviewed on 2025-11-21 14:31_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/lint.rs`:294 on 2025-11-21 14:31_

Oh no we can't because all diagnostics should remain errors on the CLI, but we want to reduce the severity of some errors to warnings in the editor only

---

_@ntBre reviewed on 2025-11-21 14:35_

---

_Review comment by @ntBre on `crates/ruff_server/src/lint.rs`:294 on 2025-11-21 14:35_

Oh right, yes. I wasn't sure if you meant could we have used the `diagnostic.severity()` in _this branch_ all along (which is what I answered), or if we could just use `diagnostic.severity()` for both cases now. I think the changes in this PR make sense to me since we do the remapping you said.

---

_@MichaReiser reviewed on 2025-11-21 14:39_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/lint.rs`:294 on 2025-11-21 14:39_

I would do a small refactor here to also show the diagnostic id in case the secondary code is missing. This should also allow us to make the two code paths more similar

```rust
let (severity, code) = if let Some(code) = code {
	(severity(code), code))
} else {
	(
		match diagnostic.severity { ... },
		&diagnostic.id
	)
};

let tags = tags(diagnostics); // or inline into the struct construction below
let code = lsp_types::NumberOrString::String(code.to_string());
```

---

_@senekor reviewed on 2025-11-21 16:07_

---

_Review comment by @senekor on `crates/ruff_server/src/lint.rs`:294 on 2025-11-21 16:07_

updated

---

_Renamed from "[ruff] Still emit diagnostic severity when secondary code is missing" to "Set severity for non-rule diagnostics" by @MichaReiser on 2025-11-21 16:33_

---

_Label `bug` added by @MichaReiser on 2025-11-21 16:34_

---

_Label `server` added by @MichaReiser on 2025-11-21 16:34_

---

_@MichaReiser approved on 2025-11-21 16:34_

Awesome, thank you

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 16:41_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Merged by @MichaReiser on 2025-11-21 16:42_

---

_Closed by @MichaReiser on 2025-11-21 16:42_

---

_Branch deleted on 2025-11-21 17:56_

---
