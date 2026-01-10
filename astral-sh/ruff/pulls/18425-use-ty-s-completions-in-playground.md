```yaml
number: 18425
title: "Use ty's completions in playground"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/playground-completions
created_at: 2025-06-02T07:49:37Z
updated_at: 2025-06-04T09:29:30Z
url: https://github.com/astral-sh/ruff/pull/18425
synced_at: 2026-01-10T18:45:04Z
```

# Use ty's completions in playground

---

_Pull request opened by @MichaReiser on 2025-06-02 07:49_

## Summary

This PR plugs in @BurntSushi's completion provider to the playground. 

CC: @BurntSushi mainly so that you see in which places you may need to make changes when adding more information to completions.


## Test Plan

This was a bit tricky to test but I noticed that `play.ty.dev` suggests `parameter` (no idea why) and we can see now that ty's completions don't suggest `parameter`.

https://github.com/user-attachments/assets/3f7dae4b-f5df-418e-ad58-217d50fcd77d





---

_Review requested from @carljm by @MichaReiser on 2025-06-02 07:49_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-02 07:49_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-02 07:49_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-02 07:49_

---

_Label `playground` added by @MichaReiser on 2025-06-02 07:51_

---

_Label `ty` added by @MichaReiser on 2025-06-02 07:51_

---

_Comment by @github-actions[bot] on 2025-06-02 07:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review request for @dcreager removed by @MichaReiser on 2025-06-02 07:58_

---

_Review request for @carljm removed by @MichaReiser on 2025-06-02 07:58_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-06-02 07:58_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-06-02 07:58_

---

_Comment by @sharkdp on 2025-06-02 15:53_

> suggests `parameter` (no idea why)

I guess it's just the word "parameter" in the *"Add parameter annotations …"* line comment.

---

_Merged by @MichaReiser on 2025-06-03 08:11_

---

_Closed by @MichaReiser on 2025-06-03 08:11_

---

_Branch deleted on 2025-06-03 08:11_

---

_Comment by @AlexWaygood on 2025-06-03 14:55_

> This was a bit tricky to test but I noticed that `play.ty.dev` suggests `parameter` (no idea why) and we can see now that ty's completions don't suggest `parameter`.

`*` imports are a great way to test this, FWIW. This is awesome! 😃

![image](https://github.com/user-attachments/assets/74c0bf03-64d9-4b3c-8e45-a5ce0df18928)

(Though it does look like there's a few symbols there such as `ReadableBuffer` that weren't actually imported by the `*` import -- they have `Definition`s in the semantic index, but the visibilities of those `Definition`s should be resolved to `AlwaysFalse` during type-checking)

---

_Comment by @sharkdp on 2025-06-04 09:29_

> (Though it does look like there's a few symbols there such as `ReadableBuffer` that weren't actually imported by the `*` import -- they have `Definition`s in the semantic index, but the visibilities of those `Definition`s should be resolved to `AlwaysFalse` during type-checking)

This will be resolved by https://github.com/astral-sh/ruff/pull/18456

---
