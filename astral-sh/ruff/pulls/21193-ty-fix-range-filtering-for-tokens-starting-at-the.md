```yaml
number: 21193
title: "[ty] Fix range filtering for tokens starting at the end of the requested range"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/semantic-tokens-fix
created_at: 2025-11-01T23:00:37Z
updated_at: 2025-11-02T14:58:37Z
url: https://github.com/astral-sh/ruff/pull/21193
synced_at: 2026-01-12T15:57:18Z
```

# [ty] Fix range filtering for tokens starting at the end of the requested range

---

_@MichaReiser_

## Summary

This PR fixes an issue where the semantic token provider returned tokens that
started at the end offset of the selected range. That is, the 
token and requested range intersect, but the intersecting range is empty. 

This isn't an issue for most documents because the only outcome is tha the server
sends one additional token to the client. However, this is an issue for notebooks
where the response can only contain tokens that all belong to the same cell. Sending one extra token can then have the effect of sending the first token of the next cell, 
which isn't allowed.

## Test Plan

Added test. My in draft notebook PR no longer panics during semantic syntax highlighting


---

_Label `server` added by @MichaReiser on 2025-11-01 23:00_

---

_Label `ty` added by @MichaReiser on 2025-11-01 23:00_

---

_Review requested from @carljm by @MichaReiser on 2025-11-01 23:00_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-11-01 23:00_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-01 23:00_

---

_Label `server` added by @MichaReiser on 2025-11-01 23:00_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-01 23:00_

---

_Label `ty` added by @MichaReiser on 2025-11-01 23:00_

---

_Comment by @github-actions[bot] on 2025-11-01 23:02_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-01 23:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-11-01 23:26_

---

_Review comment by @sharkdp on `crates/ty_ide/src/semantic_tokens.rs`:230 on 2025-11-02 13:23_

Maybe
```suggestion
            // Only include ranges that have a non-empty overlap.
```
or
```suggestion
            // Only include ranges that have a non-empty overlap. Adjacent ranges
            // should be excluded.
```


---

_Review comment by @sharkdp on `crates/ty_ide/src/semantic_tokens.rs`:1297 on 2025-11-02 13:26_

```suggestion
        // Expected: only "y" @ 7..8 and "2" @ 11..12 (non-empty overlap with target range).
```

---

_Review comment by @sharkdp on `crates/ty_ide/src/semantic_tokens.rs`:1298 on 2025-11-02 13:26_

Maybe
```suggestion
        // Not included: "1" @ 5..6 and "z" @ 13..14 (adjacent, but not overlapping at offsets 6 and 13).
```

---

_Review comment by @sharkdp on `crates/ty_ide/src/semantic_tokens.rs`:1292 on 2025-11-02 13:27_

Aside: I found the `<CURSOR>` here distracting, as I initially thought it would somehow influence or determine the target range. Can it be removed?

---

_@sharkdp approved on 2025-11-02 13:27_

Thank you!

---

_@MichaReiser reviewed on 2025-11-02 14:53_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/semantic_tokens.rs`:1292 on 2025-11-02 14:53_

I agree that `CURSOR` is ony confusing here and that semantic tokens should probably not use the cursor infrastructure to begin with. But I'd rather clean this up in a separate PR

---

_Merged by @MichaReiser on 2025-11-02 14:58_

---

_Closed by @MichaReiser on 2025-11-02 14:58_

---

_Branch deleted on 2025-11-02 14:58_

---
