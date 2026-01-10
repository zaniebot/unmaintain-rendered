```yaml
number: 16353
title: "[red-knot] Add argfile and windows glob path support"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/argfile-wild
created_at: 2025-02-24T19:02:39Z
updated_at: 2025-02-25T07:43:16Z
url: https://github.com/astral-sh/ruff/pull/16353
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Add argfile and windows glob path support

---

_Pull request opened by @MichaReiser on 2025-02-24 19:02_

## Summary

This is mostly copy pasta from Ruff:

* Adds support for argument files using `argfile` (passing CLI arguments from a file, e.g `@knot.args`). This can be useful for buck integrations or other cases where one has to pass more arguments than the shell allows 
* Adds support for glob patterns on windows (using `wild`)

## Test Plan

```
cargo run --bin red_knot -- check @knot.args
```

Where `knot.args` contains

```
--project
../test
-vv
```

runs red knot with verbose mode enabled and it checks the `../test` directory


---

_Review requested from @carljm by @MichaReiser on 2025-02-24 19:02_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-24 19:02_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-24 19:02_

---

_Label `red-knot` added by @MichaReiser on 2025-02-24 19:03_

---

_Comment by @github-actions[bot] on 2025-02-24 19:11_

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

_@carljm approved on 2025-02-24 20:07_

---

_Merged by @MichaReiser on 2025-02-25 07:43_

---

_Closed by @MichaReiser on 2025-02-25 07:43_

---

_Branch deleted on 2025-02-25 07:43_

---
