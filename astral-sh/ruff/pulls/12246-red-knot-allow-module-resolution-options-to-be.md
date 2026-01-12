```yaml
number: 12246
title: "[red-knot] Allow module-resolution options to be specified via the CLI"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: redknot-module-resolver-args
created_at: 2024-07-08T16:39:37Z
updated_at: 2024-07-09T09:30:47Z
url: https://github.com/astral-sh/ruff/pull/12246
synced_at: 2026-01-12T15:55:40Z
```

# [red-knot] Allow module-resolution options to be specified via the CLI

---

_@AlexWaygood_

## Summary

This allows us to pass arguments such as `--custom-typeshed-dir` and `--target-version` directly to the red-knot CLI, for easier experimentation and validation locally.

(The PR is stacked on top of https://github.com/astral-sh/ruff/pull/12215)

## Test Plan

Lots of manual testing, for example:

<details>
<summary>Screenshots</summary>

![image](https://github.com/astral-sh/ruff/assets/66076021/184fd733-7faa-4df9-9362-d9c4f1756fb2)

---

![image](https://github.com/astral-sh/ruff/assets/66076021/ec8da209-8a91-4bec-8ba2-9e2174cb0298)

</details>


---

_Label `red-knot` added by @AlexWaygood on 2024-07-08 16:39_

---

_Review requested from @carljm by @AlexWaygood on 2024-07-08 16:39_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-08 16:39_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/Cargo.toml`:17 on 2024-07-08 16:50_

Hmm, this is the part that annoys me about clap. It shouldn't be necessary to have a cli dependency (and such a heavy one) on non cli crates. 

Ideally we have the same enum in red knot (we'll need it for other things as well). Alternatively, add a clap feature and make it an optional dependency 

---

_Comment by @MichaReiser on 2024-07-08 16:50_

The summary is half honest haha. The biggest change is that this is pr migrates to Clap 😂

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:36 on 2024-07-08 16:51_

Do we need the environment variable? I would prefer to start with the absolute minimum 

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:66 on 2024-07-08 16:53_

This trace feels a bit extra verbose. I would remove it. 

---

_@MichaReiser approved on 2024-07-08 16:54_

Migrating to clap was something on my todo list. Thanks for doing for me

---

_Review comment by @AlexWaygood on `crates/red_knot/src/main.rs`:66 on 2024-07-08 16:57_

Just this line, or the other `tracing::trace!()` lines I added as well?

---

_@AlexWaygood reviewed on 2024-07-08 16:57_

---

_Comment by @github-actions[bot] on 2024-07-08 16:58_

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

_@AlexWaygood reviewed on 2024-07-08 16:58_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/main.rs`:36 on 2024-07-08 16:58_

no, we can leave this for now. though I expect we will want to let users specify it via environment variable as well at some point

---

_Comment by @AlexWaygood on 2024-07-08 16:59_

> The summary is half honest haha. The biggest change is that this is pr migrates to Clap

just a means to an end ;)

---

_@MichaReiser reviewed on 2024-07-08 17:03_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:66 on 2024-07-08 17:03_

Mainly that one. We probably want to move the others further down but I think they're fine 

---

_Review comment by @carljm on `crates/red_knot/src/target_version.rs`:7 on 2024-07-08 19:18_

Why do we need this defined both here and in `red_knot_module_resolver` crate?

---

_@carljm approved on 2024-07-08 19:18_

Nice!

---

_@AlexWaygood reviewed on 2024-07-08 19:22_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/target_version.rs`:7 on 2024-07-08 19:22_

I originally just imported it from `red_knot_module_resolver`, but Micha suggested duplicating it here so that we can avoid having `clap` as a dependency in `red_knot_module_resolver`: https://github.com/astral-sh/ruff/pull/12246#discussion_r1668970240. For the version of the enum that's used as a `clap` argument, it needs to have `ValueEnum` derived on it, which means that the crate in which it's defined needs to have `clap` as a dependency

---

_Merged by @AlexWaygood on 2024-07-09 09:16_

---

_Closed by @AlexWaygood on 2024-07-09 09:16_

---

_Branch deleted on 2024-07-09 09:16_

---
