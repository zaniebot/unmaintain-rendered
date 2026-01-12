```yaml
number: 17657
title: "[red-knot] Add support for the LSP diagnostic tag"
type: pull_request
state: merged
author: ercbot
labels:
  - ty
assignees: []
merged: true
base: main
head: eb/diagnostic-tag
created_at: 2025-04-27T20:58:32Z
updated_at: 2025-05-03T18:35:04Z
url: https://github.com/astral-sh/ruff/pull/17657
synced_at: 2026-01-12T15:56:03Z
```

# [red-knot] Add support for the LSP diagnostic tag

---

_@ercbot_

## Summary

closes #17536

- Adds `DiagnosticTag enum { Unnecessary, Deprecated }` and a tags fields to the Annotation struct in the ruff diagnostics system
- Updates the `to_lsp_diagnostic` function to include conversion from the above tags to LSP Diagnostic Tags. 

Note: doesn't update any existing code to use the tags, just wires it up for future implementation

## Test Plan

Existing tests have passed, seeking guidance on where to add new tests for this if necessary 

---

_Label `red-knot` added by @AlexWaygood on 2025-04-27 21:30_

---

_Comment by @github-actions[bot] on 2025-04-27 21:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review requested from @BurntSushi by @MichaReiser on 2025-04-28 06:12_

---

_Comment by @MichaReiser on 2025-04-28 06:13_

Nice!

> Existing tests have passed, seeking guidance on where to add new tests for this if necessary

We don't really have tests for our diagnostic conversion code. So I thinkit's fine to skip automated tests and instead test this manually (you can change an arbitrary diagnostic to add the `DEPRECATED` or `UNNECESSARY` tag)

---

_@BurntSushi approved on 2025-04-28 14:18_

LGTM

---

_Marked ready for review by @ercbot on 2025-04-29 03:23_

---

_Review requested from @carljm by @ercbot on 2025-04-29 03:23_

---

_Review requested from @MichaReiser by @ercbot on 2025-04-29 03:23_

---

_Review requested from @AlexWaygood by @ercbot on 2025-04-29 03:23_

---

_Review requested from @sharkdp by @ercbot on 2025-04-29 03:23_

---

_Review requested from @dcreager by @ercbot on 2025-04-29 03:23_

---

_Comment by @MichaReiser on 2025-04-29 05:56_

This is great. Would you mind adding a test plan to your PR summary? What I do for features like this is that I record a screen recording demonstrating the feature in VS code. 

This will require adding the tag to at least one emitted diagnostic.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-29 06:40_

---

_Comment by @dhruvmanila on 2025-04-29 20:27_

If you're using VS Code, you can use the existing Ruff extension to test out the red-knot server by updating [`ruff.path`](https://docs.astral.sh/ruff/editors/settings/#path) to point to the `red_knot` binary:

```json
{
	"ruff.path": ["/path/to/red_knot"]
}
```

---

_Comment by @MichaReiser on 2025-05-03 18:26_

I went ahead and rebased this PR, added methods to set the tags on `Diagnostic` and tested that the tag is propagated to the client by adding it to `DIVISION_BY_ZERO` (see how the code is now grayed out in VS code)

<img width="777" alt="Screenshot 2025-05-03 at 20 25 01" src="https://github.com/user-attachments/assets/dba27fab-15a6-4e51-96a6-fb9bcdf39635" />


---

_Merged by @MichaReiser on 2025-05-03 18:35_

---

_Closed by @MichaReiser on 2025-05-03 18:35_

---
