```yaml
number: 17002
title: "[red-knot] Add run panel"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/playground-run
created_at: 2025-03-26T19:18:04Z
updated_at: 2025-03-26T21:39:20Z
url: https://github.com/astral-sh/ruff/pull/17002
synced_at: 2026-01-12T15:55:59Z
```

# [red-knot] Add run panel

---

_@MichaReiser_

## Summary

This PR adds a new secondary panel to the red knot playground that allows running the python code (current file) with [pyodide](https://pyodide.org/en/stable/index.html) (currently Python 3.12 only). 



## Test Plan

https://github.com/user-attachments/assets/7bda8ef7-19fb-4c2f-8e62-8e49a1416be1



---

_Label `playground` added by @MichaReiser on 2025-03-26 19:18_

---

_Label `red-knot` added by @MichaReiser on 2025-03-26 19:18_

---

_Review requested from @Copilot by @MichaReiser on 2025-03-26 20:50_

---

_@copilot-pull-request-reviewer[bot] reviewed on 2025-03-26 20:51_

## Pull Request Overview

This pull request adds a new run panel for executing Python code via pyodide, along with the related UI changes.  
- Updated Vite configuration to copy pyodide assets and exclude them from dependency optimization.  
- Introduced a new Run icon and integrated the Run panel into the SecondaryPanel and Sidebar.  
- Modified the initialization logic in the Playground to support dynamic loading of the red_knot wasm module.

### Reviewed Changes

Copilot reviewed 6 out of 8 changed files in this pull request and generated 1 comment.

<details>
<summary>Show a summary per file</summary>

| File                             | Description                                                            |
| -------------------------------- | ---------------------------------------------------------------------- |
| playground/knot/vite.config.ts   | Added pyodide asset copying and configuration adjustments              |
| playground/shared/src/Icons.tsx   | Introduced a new Run icon SVG                                          |
| playground/knot/src/Editor/SecondaryPanel.tsx | Added Run tool integration with pyodide loading and run functionality    |
| playground/knot/src/Editor/SecondarySideBar.tsx | Added Run entry to the sidebar using the new icon                         |
| playground/knot/src/Editor/Chrome.tsx          | Updated panel integration to include Run tool behavior                    |
| playground/knot/src/Playground.tsx             | Changed initialization logic via dynamic import of the red_knot wasm module |
</details>


<details>
<summary>Files not reviewed (2)</summary>

* **playground/knot/package.json**: Language not supported
* **playground/package-lock.json**: Language not supported
</details>




---

_Review comment by @Copilot on `playground/knot/vite.config.ts`:23 on 2025-03-26 20:51_

Remove or conditionally execute the console.log statement to avoid production-level logging.
```suggestion
  if (process.env.NODE_ENV !== 'production') {
    console.log(pyodideDir, PYODIDE_EXCLUDE);
  }
```

---

_Marked ready for review by @MichaReiser on 2025-03-26 20:58_

---

_Merged by @MichaReiser on 2025-03-26 21:32_

---

_Closed by @MichaReiser on 2025-03-26 21:32_

---

_Branch deleted on 2025-03-26 21:32_

---

_@sharkdp reviewed on 2025-03-26 21:39_

---

_Review comment by @sharkdp on `playground/knot/src/Editor/SecondaryPanel.tsx`:155 on 2025-03-26 21:39_

The patched version of `reveal_type` that I'd prefer would be something like:
```py
def reveal_type(obj):
    print(f"Runtime value is '{obj}'")
    import typing
    return typing.reveal_type(obj)
```
We should definitely have something that still return the original value, otherwise we'll break some code that uses `reveal_type` on intermediate expressions.

---
