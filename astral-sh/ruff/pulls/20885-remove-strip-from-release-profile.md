```yaml
number: 20885
title: "Remove `strip` from release profile"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/remove-strip
created_at: 2025-10-15T09:31:31Z
updated_at: 2025-10-15T09:48:55Z
url: https://github.com/astral-sh/ruff/pull/20885
synced_at: 2026-01-12T15:57:11Z
```

# Remove `strip` from release profile

---

_@MichaReiser_

## Summary

I added `strip="symbols"` in https://github.com/astral-sh/ruff/pull/20863 for consistency with our released binaries (we always set strip=true when building with maturin).

Unfortunately, this breaks the WASM build (https://github.com/rust-lang/rust/issues/93294). I don't think it's worth having a separate release target for WASM only and 
`strip=symbols` isn't needed to fix the binary regression. 

## Test Plan

`npm run build --workspace ty-playground` compiles without error



---

_Label `internal` added by @MichaReiser on 2025-10-15 09:31_

---

_Merged by @MichaReiser on 2025-10-15 09:36_

---

_Closed by @MichaReiser on 2025-10-15 09:36_

---

_Branch deleted on 2025-10-15 09:36_

---

_Comment by @github-actions[bot] on 2025-10-15 09:48_

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
