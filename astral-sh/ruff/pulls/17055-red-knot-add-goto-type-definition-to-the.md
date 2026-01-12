```yaml
number: 17055
title: "[red-knot] Add 'Goto type definition' to the playground"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/playground-goto-type-definition
created_at: 2025-03-29T11:25:20Z
updated_at: 2025-04-02T14:35:34Z
url: https://github.com/astral-sh/ruff/pull/17055
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Add 'Goto type definition' to the playground

---

_@MichaReiser_

## Summary

This PR adds Goto type definition to the playground, using the same infrastructure as the LSP. 


The main *challenge* with implementing this feature was that the editor can now participate in which tab is open. 

## Known limitations

The same as for the LSP. Most notably, navigating to types defined in typeshed isn't supported.

## Test Plan

https://github.com/user-attachments/assets/22dad7c8-7ac7-463f-b066-5d5b2c45d1fe






---

_Label `playground` added by @MichaReiser on 2025-03-29 11:25_

---

_Label `red-knot` added by @MichaReiser on 2025-03-29 11:25_

---

_Comment by @github-actions[bot] on 2025-03-29 11:27_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-04-01 11:41_

---

_Review requested from @carljm by @MichaReiser on 2025-04-01 11:41_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-01 11:41_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-01 11:41_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-01 11:41_

---

_Comment by @MichaReiser on 2025-04-01 11:42_

Where is the github co-pilot reviewer when you need it.... 

---

_Review requested from @Copilot by @MichaReiser on 2025-04-01 11:42_

---

_@copilot-pull-request-reviewer[bot] reviewed on 2025-04-01 11:42_

## Pull Request Overview

This PR adds a “Goto type definition” feature to the playground by leveraging the existing LSP infrastructure and updating both the frontend editor and the Rust backend.  
- Introduces a helper method to detect Python files in Files.tsx.  
- Updates the Editor.tsx to register a type definition provider and handle file model creation and switching.  
- Adds a new Rust function in red_knot_wasm to power go-to-type-definition and updates dependency configuration.

### Reviewed Changes

Copilot reviewed 7 out of 7 changed files in this pull request and generated no comments.

<details>
<summary>Show a summary per file</summary>

| File                                             | Description                                                            |
| ------------------------------------------------ | ---------------------------------------------------------------------- |
| playground/knot/src/Editor/Files.tsx             | Added helper function to check if a file is a Python file               |
| playground/knot/src/Editor/Editor.tsx              | Updated editor API to support type definition operations and file switching |
| playground/knot/src/Editor/Chrome.tsx              | Modified integration to pass new props to the editor                   |
| crates/red_knot_wasm/src/lib.rs                    | Added Rust function for type definition lookup and enhanced range conversion |
| crates/red_knot_wasm/Cargo.toml                    | Added dependency on red_knot_ide                                         |
| crates/red_knot_ide/src/lib.rs                     | Implemented file_range accessor for improved integration                |
</details>



<details>
<summary>Comments suppressed due to low confidence (2)</summary>

**playground/knot/src/Editor/Editor.tsx:210**
* [nitpick] The variable name 'selectedFile' might be misleading if it represents a file identifier. Consider renaming it to 'selectedFileId' for enhanced clarity.
```
const selectedFile = this.props.current.files.selected;
```
**playground/knot/src/Editor/Editor.tsx:260**
* [nitpick] The name 'fileId' might benefit from additional context (e.g. 'targetFileId') to clarify that it is derived from a file lookup based on the resource URI.
```
const fileId = files.index.find((file) => { return Uri.file(file.name).toString() === resource.toString(); })?.id;
```
</details>



---

_Comment by @github-actions[bot] on 2025-04-01 11:49_

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

_Review requested from @dhruvmanila by @MichaReiser on 2025-04-01 16:19_

---

_Merged by @MichaReiser on 2025-04-02 14:35_

---

_Closed by @MichaReiser on 2025-04-02 14:35_

---

_Branch deleted on 2025-04-02 14:35_

---
