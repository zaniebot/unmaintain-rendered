```yaml
number: 17007
title: "[red-knot] `reveal-type` should return the revaled type"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/reveal-type
created_at: 2025-03-27T02:45:51Z
updated_at: 2025-03-27T03:04:59Z
url: https://github.com/astral-sh/ruff/pull/17007
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] `reveal-type` should return the revaled type

---

_@MichaReiser_

## Summary

Return the revealed-type from the monkey-patched `revale_type` implementation to 
preserve the identity behavior.

This PR also isolates different script runs by assigning a different `globals` dict for each script-run. See https://github.com/pyodide/pyodide/issues/703


---

_Label `playground` added by @MichaReiser on 2025-03-27 02:46_

---

_Label `red-knot` added by @MichaReiser on 2025-03-27 02:46_

---

_Review requested from @Copilot by @MichaReiser on 2025-03-27 02:57_

---

_@copilot-pull-request-reviewer[bot] reviewed on 2025-03-27 02:57_

## Pull Request Overview

This PR enhances the Python script execution in the playground by patching the reveal_type behavior to return the revealed type and by isolating script runs with a separate globals dictionary. It also updates file path specifications in the workflow configuration to include all nested changes.
- Updated SecondaryPanel.tsx to capture the selected file name and apply a custom reveal_type function that prints the runtime value and returns its type.
- Isolated the globals for each script-run and ensured proper destruction of proxy objects.
- Modified the workflow file to use recursive path patterns for directory changes.

### Reviewed Changes

Copilot reviewed 2 out of 2 changed files in this pull request and generated 1 comment.

| File                                           | Description                                                                              |
| ---------------------------------------------- | ---------------------------------------------------------------------------------------- |
| playground/knot/src/Editor/SecondaryPanel.tsx  | Enhances the reveal_type patch and isolates globals for each Python script-run execution. |
| .github/workflows/publish-knot-playground.yml  | Adjusts file path patterns to include nested directories.                                |





---

_Review comment by @Copilot on `playground/knot/src/Editor/SecondaryPanel.tsx`:166 on 2025-03-27 02:57_

The usage of backticks inside the f-string may lead to unintended formatting issues when the string is passed within a template literal. Consider using alternative quoting (for example, single quotes inside the f-string) to avoid potential syntax errors.

---

_Merged by @MichaReiser on 2025-03-27 03:04_

---

_Closed by @MichaReiser on 2025-03-27 03:04_

---

_Branch deleted on 2025-03-27 03:04_

---
