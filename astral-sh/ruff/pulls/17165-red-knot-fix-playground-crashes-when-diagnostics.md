```yaml
number: 17165
title: "[red-knot] Fix playground crashes when diagnostics are stale"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/playground-fix-stale-diagnostics
created_at: 2025-04-03T08:13:01Z
updated_at: 2025-04-03T08:18:38Z
url: https://github.com/astral-sh/ruff/pull/17165
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Fix playground crashes when diagnostics are stale

---

_@MichaReiser_

## Summary

Fixes a crash in the playground where it crashed with an "index out of bounds" error in the `Diagnostic::to_range` call 
after deleting content at the end of the file. 

The root cause was that the playground uses `useDeferred` to avoid too frequent `checkFile` calls (to get a smoother UX). 
However, this has the problem that the rendered `diagnostics` can be stable (from before the last change). 
Rendering the diagnostics can then fail because the `toRange` call queries the latest content and not the content
from when the diagnostics were created.

The fix is "easy" in the sense that we now eagerly perform the `toRange` calls. This way, it doesn't matter
when the diagnostics are stale for a few ms. 

This problem can only be observed on examples where Red Knot is "slow" (takes more than ~16ms to check) because
only then does `useDeferred` "debounce" the `check` calls.




---

_Label `playground` added by @MichaReiser on 2025-04-03 08:13_

---

_Label `red-knot` added by @MichaReiser on 2025-04-03 08:13_

---

_Review requested from @Copilot by @MichaReiser on 2025-04-03 08:13_

---

_Review comment by @Copilot on `playground/knot/src/Editor/Chrome.tsx`:295 on 2025-04-03 08:14_

The Diagnostic API now defines 'id' as a property rather than a method. Please update 'diagnostic.id()' to 'diagnostic.id'.
```suggestion
        id: diagnostic.id,
```

---

_@copilot-pull-request-reviewer[bot] reviewed on 2025-04-03 08:14_

## Pull Request Overview

This PR fixes a crash in the playground caused by stale diagnostics by eagerly converting diagnostic information.  
- Replace method calls with property accesses for diagnostic fields.  
- Rename the file open callback from onOpenFile to onFileOpened across the code.  
- Eagerly serialize diagnostics in Chrome.tsx for improved error handling.

### Reviewed Changes

Copilot reviewed 3 out of 3 changed files in this pull request and generated 3 comments.

| File                                        | Description                                                    |
| ------------------------------------------- | -------------------------------------------------------------- |
| playground/knot/src/Editor/Editor.tsx       | Updated diagnostic and file open callback handling.            |
| playground/knot/src/Editor/Diagnostics.tsx  | Changed diagnostic property accesses and removed workspace use.|
| playground/knot/src/Editor/Chrome.tsx         | Renamed callback prop and updated diagnostic conversion logic.   |





---

_Review comment by @Copilot on `playground/knot/src/Editor/Chrome.tsx`:298 on 2025-04-03 08:14_

The new Diagnostic API exposes 'range' as a property instead of using the 'toRange' method. Update this line to use 'diagnostic.range' instead.
```suggestion
        range: diagnostic.range ?? null,
```

---

_Review comment by @Copilot on `playground/knot/src/Editor/Chrome.tsx`:299 on 2025-04-03 08:14_

Similarly, 'textRange' should be accessed as a property rather than a method. Change 'diagnostic.textRange()' to 'diagnostic.textRange'.
```suggestion
        textRange: diagnostic.textRange ?? null,
```

---

_Comment by @MichaReiser on 2025-04-03 08:15_

Copilot oversimplifies things here. We still need to call the WASM methods and that requires using `id()` etc.

---

_Merged by @MichaReiser on 2025-04-03 08:18_

---

_Closed by @MichaReiser on 2025-04-03 08:18_

---

_Branch deleted on 2025-04-03 08:18_

---
