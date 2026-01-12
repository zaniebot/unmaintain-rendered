```yaml
number: 12681
title: Red Knot Playground
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: red-knot-playground
created_at: 2024-08-05T07:12:05Z
updated_at: 2025-03-18T16:19:43Z
url: https://github.com/astral-sh/ruff/pull/12681
synced_at: 2026-01-12T15:55:41Z
```

# Red Knot Playground

---

_@MichaReiser_

## Summary

This PR adds a playground for Red Knot

[Screencast from 2024-08-14 10-33-54.webm](https://github.com/user-attachments/assets/ae81d85f-74a3-4ba6-bb61-4a871b622f05)

Sharing does work :laughing: I just forgot to start wrangler. 


It supports:

* Multiple files
* Showing the AST
* Showing the tokens
* Sharing
* Persistence to local storage

Future extensions:

* Configuration support: The `pyproject.toml` would *just* be another file. 
* Showing type information on hover

## Blockers

~~Salsa uses `catch_unwind` to break cycles, which Red Knot uses extensively when inferring types in the standard library. 
However, WASM (at least `wasm32-unknown-unknown`) doesn't support `catch_unwind` today, so the playground always crashes when the type inference encounters a cycle.~~

~~I created a discussion in the [salsa zulip](https://salsa.zulipchat.com/#narrow/stream/333573-salsa-3.2E0/topic/WASM.20support) to see if it would be possible to **not** use catch unwind to break cycles.~~

~~[Rust tracking issue for WASM catch unwind support](https://github.com/rust-lang/rust/issues/118168)~~

~~I tried to build the WASM with the nightly compiler option but ran into problems because wasm-bindgen doesn't support WASM-exceptions. We could try to write the binding code by hand.~~

~~Another alternative is to use `wasm32-unknown-emscripten` but it's rather painful to build~~


---

_Label `red-knot` added by @MichaReiser on 2024-08-10 15:35_

---

_Comment by @codspeed-hq[bot] on 2024-08-11 15:09_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/red-knot-playground)

### Merging #12681 will **not alter performance**

<sub>Comparing <code>red-knot-playground</code> (2f7c8e3) with <code>main</code> (dcf31c9)</sub>



### Summary

`✅ 32` untouched benchmarks  





---

_Comment by @github-actions[bot] on 2024-08-11 15:18_

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

_Renamed from "[WIP] Red Knot Playground" to "Red Knot Playground" by @MichaReiser on 2024-08-14 08:23_

---

_@MichaReiser reviewed on 2024-08-14 08:29_

---

_Review comment by @MichaReiser on `crates/red_knot_wasm/src/lib.rs`:35 on 2024-08-14 08:29_

Add a URl param to determine the log level: `?tracing=debug`

---

_Comment by @TomerBin on 2024-10-01 20:29_

Hey! Any updates about this? Do we have any sort of playground for red knot at the moment? 

---

_Comment by @MichaReiser on 2024-10-07 07:32_

No, there's no update. This is blocked on adding WASM support to salsa. See https://salsa.zulipchat.com/#narrow/stream/333573-Contributing-to-Salsa/topic/WASM.20support

---

_Comment by @MichaReiser on 2025-03-13 07:47_

I plan to pick this up again on the side. It's now unblocked as we use salsa's new fixpoint infrastructure that doesn't rely on panics to resolve cycles.

---

_Label `playground` added by @MichaReiser on 2025-03-18 10:32_

---

_Comment by @MichaReiser on 2025-03-18 12:32_

Weird, I can't reproduce this error locally

---

_Comment by @github-actions[bot] on 2025-03-18 14:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-03-18 16:16_

---

_Review requested from @carljm by @MichaReiser on 2025-03-18 16:16_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-03-18 16:16_

---

_Review requested from @sharkdp by @MichaReiser on 2025-03-18 16:16_

---

_Review requested from @dcreager by @MichaReiser on 2025-03-18 16:16_

---

_Merged by @MichaReiser on 2025-03-18 16:17_

---

_Closed by @MichaReiser on 2025-03-18 16:17_

---

_Branch deleted on 2025-03-18 16:17_

---
