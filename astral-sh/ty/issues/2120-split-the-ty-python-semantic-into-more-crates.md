---
number: 2120
title: "Split the `ty_python_semantic` into more crates"
type: issue
state: closed
author: MichaReiser
labels:
  - internal
assignees: []
created_at: 2025-12-19T20:47:12Z
updated_at: 2025-12-25T07:29:11Z
url: https://github.com/astral-sh/ty/issues/2120
synced_at: 2026-01-10T01:52:52Z
---

# Split the `ty_python_semantic` into more crates

---

_Issue opened by @MichaReiser on 2025-12-19 20:47_

The `ty_python_semantic` crates has become pretty big and slow to compile. Splitting the type inference logic is tricky but we could try:

* Extract the module resolver into `ty_module_resolver`: 
  * Create a new `Db` in the resolver crate with a `search_paths` method (to remove the dependency on `Program`)
  *  Create a new `TestDb` for the module resolver
  * Create a `SearchPathBuilder` to replace `Program::from_settings`. 
  * I'm sure there are some more wrinkles to it
* Extract `ty_python_infer` for the type inference logic. This seems much trickier. Not sure if it's worth it

---

_Label `internal` added by @MichaReiser on 2025-12-19 20:47_

---

_Closed by @MichaReiser on 2025-12-21 11:00_

---

_Comment by @AlexWaygood on 2025-12-24 13:11_

Other parts of ty_python_semantic that could make sense to split out are:
- `list.rs`
- `rank.rs`
- (Maybe slightly trickier) `site_packages.rs`

---

_Comment by @mtshiba on 2025-12-24 21:12_

This is a slightly different idea, but when building ty, if we only want the type checker, maybe we can remove `ty_server` from the dependencies (dependencies can be toggled using feature flags).
I often rebuild ty after making changes to `ty_python_semantic` just to observe the change in behavior, and in this case, the language server is not necessary. Even when running ty on CI, there may be times when we do not need the language server's functionality.

---

_Comment by @MichaReiser on 2025-12-25 07:29_

<img width="2485" height="1576" alt="Image" src="https://github.com/user-attachments/assets/b1917de2-fec8-408a-b13b-9c2eae826c69" />

Looking at the timing report, I'm not sure if splitting out `ty_server` gives us that much. The dominant time really is `ty_python_semantic`. I created https://github.com/astral-sh/ty/issues/2199 to analysis why compiling `ty_python_semantic` takes that long

---
