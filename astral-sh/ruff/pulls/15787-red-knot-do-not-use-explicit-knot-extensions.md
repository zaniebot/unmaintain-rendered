```yaml
number: 15787
title: "[red-knot] Do not use explicit `knot_extensions.Unknown` declaration"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/use-implicit-unknown-declaration
created_at: 2025-01-28T15:56:51Z
updated_at: 2025-01-28T16:18:24Z
url: https://github.com/astral-sh/ruff/pull/15787
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Do not use explicit `knot_extensions.Unknown` declaration

---

_Pull request opened by @sharkdp on 2025-01-28 15:56_

## Summary

Do not use an explict `knot_extensions.Unknown` declaration, as per [this comment](https://github.com/astral-sh/ruff/pull/15766#discussion_r1930997592). Instead, use an undefined name to achieve the same effect.



---

_Label `red-knot` added by @sharkdp on 2025-01-28 15:56_

---

_Review requested from @carljm by @sharkdp on 2025-01-28 15:56_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-28 15:56_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-28 15:56_

---

_@sharkdp reviewed on 2025-01-28 15:58_

---

_Review comment by @sharkdp on `crates/red_knot_test/README.md`:286 on 2025-01-28 15:58_

Drive-by fix, `-q` is not needed anymore to silence the "Reading inline script metadata from …" message since https://github.com/astral-sh/uv/pull/10588

---

_@carljm approved on 2025-01-28 16:06_

---

_Merged by @sharkdp on 2025-01-28 16:18_

---

_Closed by @sharkdp on 2025-01-28 16:18_

---

_Branch deleted on 2025-01-28 16:18_

---
