```yaml
number: 17223
title: "[red-knot] Simplify playground editor state"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/playground-simplify-editor
created_at: 2025-04-05T17:43:34Z
updated_at: 2025-04-05T17:49:09Z
url: https://github.com/astral-sh/ruff/pull/17223
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Simplify playground editor state

---

_Pull request opened by @MichaReiser on 2025-04-05 17:43_

## Summary
Reduce the number of `useRef`s in `Editor`




---

_Label `playground` added by @MichaReiser on 2025-04-05 17:43_

---

_Label `red-knot` added by @MichaReiser on 2025-04-05 17:43_

---

_Review requested from @Copilot by @MichaReiser on 2025-04-05 17:43_

---

_Review comment by @Copilot on `playground/knot/src/Editor/Editor.tsx`:65 on 2025-04-05 17:44_

Updating the server properties directly inside the component body may cause unintended side effects during rendering; consider moving this update into a useEffect hook with [files, workspace, onFileOpened] as dependencies.

---

_@copilot-pull-request-reviewer[bot] reviewed on 2025-04-05 17:44_



Copilot reviewed 1 out of 1 changed files in this pull request and generated 1 comment.


<details>
<summary>Comments suppressed due to low confidence (1)</summary>

**playground/knot/src/Editor/Editor.tsx:178**
* Parsing the file path as a URI using Uri.parse assumes that the path is always a valid URI; consider adding validation or error handling to avoid potential runtime issues.
```
const model = editor.getModel(Uri.parse(handle.path()));
```
</details>



---

_Merged by @MichaReiser on 2025-04-05 17:49_

---

_Closed by @MichaReiser on 2025-04-05 17:49_

---

_Branch deleted on 2025-04-05 17:49_

---
