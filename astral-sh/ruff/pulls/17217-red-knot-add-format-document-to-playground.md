```yaml
number: 17217
title: "[red-knot] Add 'Format document' to playground"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/playground-format
created_at: 2025-04-05T08:12:23Z
updated_at: 2025-04-07T07:29:32Z
url: https://github.com/astral-sh/ruff/pull/17217
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Add 'Format document' to playground

---

_Pull request opened by @MichaReiser on 2025-04-05 08:12_

## Summary
This is more "because we can" than something we need. 

But since we're already building an "almost IDE" 

## Test Plan


https://github.com/user-attachments/assets/3a4bdad1-ba32-455a-9909-cfeb8caa1b28



---

_Label `playground` added by @MichaReiser on 2025-04-05 08:13_

---

_Label `red-knot` added by @MichaReiser on 2025-04-05 08:13_

---

_Marked ready for review by @MichaReiser on 2025-04-05 08:13_

---

_Review requested from @carljm by @MichaReiser on 2025-04-05 08:13_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-05 08:13_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-05 08:13_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-05 08:13_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-04-05 08:13_

---

_Comment by @github-actions[bot] on 2025-04-05 08:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser reviewed on 2025-04-05 08:16_

---

_Review comment by @MichaReiser on `crates/red_knot_wasm/src/lib.rs`:196 on 2025-04-05 08:16_

I considered making `ruff_python_formatter` defining its own DB and expose a `format` query that returns `Formatted` or Unformatted(String)` but it seemed more work because it requires introducing a db trait and what not and I'm not sure if this is indeed the query API that we want. 

For now, this is ad-hoc, but it re-uses the cached AST (although we need to clone it because of API incompatibilities :()

---

_@MichaReiser reviewed on 2025-04-05 08:17_

---

_Review comment by @MichaReiser on `playground/knot/src/Editor/Editor.tsx`:378 on 2025-04-05 08:17_

We could be more fancy and change `format` to return a vec of text edits. I don't think we need this just yet (also comes at the cost of requiring a diff library)

---

_Review requested from @Copilot by @MichaReiser on 2025-04-05 08:17_

---

_@copilot-pull-request-reviewer[bot] reviewed on 2025-04-05 08:17_



Copilot reviewed 5 out of 5 changed files in this pull request and generated no comments.


<details>
<summary>Comments suppressed due to low confidence (1)</summary>

**playground/knot/src/Editor/Editor.tsx:366**
* In the provideDocumentFormattingEdits implementation, consider explicitly returning null (instead of implicitly returning undefined) when no file is selected, to ensure consistent return types as expected by the formatting provider.
```
if (this.props.current.files.selected == null) {
```
</details>



---

_Comment by @github-actions[bot] on 2025-04-05 08:21_

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

_Review request for @dcreager removed by @MichaReiser on 2025-04-05 14:35_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-04-05 14:35_

---

_Review request for @carljm removed by @carljm on 2025-04-06 19:52_

---

_@charliermarsh approved on 2025-04-07 02:03_

---

_Merged by @MichaReiser on 2025-04-07 07:26_

---

_Closed by @MichaReiser on 2025-04-07 07:26_

---

_Branch deleted on 2025-04-07 07:26_

---
