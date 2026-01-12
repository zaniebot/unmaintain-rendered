```yaml
number: 20263
title: "[ty] Include `python` folder in `environment.root` if it exists"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-1120
created_at: 2025-09-05T09:34:52Z
updated_at: 2025-09-05T11:53:50Z
url: https://github.com/astral-sh/ruff/pull/20263
synced_at: 2026-01-12T15:56:57Z
```

# [ty] Include `python` folder in `environment.root` if it exists

---

_@sharkdp_

## Summary

I felt it was safer to add the `python` folder *in addition* to a possibly-existing `src` folder, even though the `src` folder only contains Rust code for `maturin`-based projects. There might be non-maturin projects where a `python` folder exists for other reasons, next to a normal `src` layout.

closes https://github.com/astral-sh/ty/issues/1120

## Test Plan

Tested locally on the egglog-python project.


---

_Review requested from @carljm by @sharkdp on 2025-09-05 09:34_

---

_Review requested from @MichaReiser by @sharkdp on 2025-09-05 09:34_

---

_Review requested from @dcreager by @sharkdp on 2025-09-05 09:34_

---

_Label `ty` added by @sharkdp on 2025-09-05 09:34_

---

_Comment by @github-actions[bot] on 2025-09-05 09:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-05 09:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_project/src/metadata/options.rs`:263 on 2025-09-05 10:21_

I think we should do a similar thing to what we do with the `tests/` directory immediately below:
- Only add it to `roots` if it wasn't already explicitly specified by the user (otherwise we might end up with duplicate search paths, I guess?)
- Only add it to `roots` if `./python/__init__.py` does _not_ exist (if it _does_ exist, it's fairly clear that it's meant to be a top-level package rather than a first-party search path)

The existing `tests/` logic should probably also check for `./tests/__init__.pyi` as well as `./tests/__init__.py`. You could possibly also fix that while we're here, and also check for ./python/__init__.pyi` as well as `./python/__init__.py`.

---

_Review comment by @AlexWaygood on `crates/ty_project/src/metadata/options.rs`:265 on 2025-09-05 10:21_

```suggestion
                // for maturin-based rust/python projects [1].
```

---

_@AlexWaygood reviewed on 2025-09-05 10:23_

Nice. I think it's probably also worth adding a unit tests, and expanding the docs, similar to what was done in https://github.com/astral-sh/ruff/commit/97ff015c88354b5d45b52a9b1460c30978b5c29b

---

_Comment by @AlexWaygood on 2025-09-05 10:27_

After landing this, we should also update the docs at https://github.com/astral-sh/ty/blob/main/docs/modules.md#first-party-modules. The docs at https://github.com/astral-sh/ty/blob/main/docs/reference/configuration.md#root don't need to be manually updated; they're generated from the doc-comment on the `Options` struct.

---

_@AlexWaygood approved on 2025-09-05 11:45_

Thanks!

---

_Merged by @sharkdp on 2025-09-05 11:53_

---

_Closed by @sharkdp on 2025-09-05 11:53_

---

_Branch deleted on 2025-09-05 11:53_

---
