```yaml
number: 20715
title: "[ty] Enforce that `typing_extensions` must come from a stdlib search path"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/typing-extensions-shadowing
created_at: 2025-10-06T10:03:42Z
updated_at: 2025-10-06T11:43:35Z
url: https://github.com/astral-sh/ruff/pull/20715
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Enforce that `typing_extensions` must come from a stdlib search path

---

_Pull request opened by @AlexWaygood on 2025-10-06 10:03_

## Summary

This fixes the panic in https://github.com/astral-sh/ty/issues/1289. It's also what mypy does, and it seems like reasonable behaviour. (I don't want to close that issue, though, because I think we should do other things as well, such as improve our documentation around these options.)

## Test Plan

The following steps reproduce the panic on `main`:

1. `git clone https://github.com/copdips/ty-issues-1289`
2. `uv venv -p3.12`
3. `uv sync`
4. `cd backend`
5. `cargo run --manifest-path path/to/ruff/clone -p ty check`

If I then checkout this branch on my Ruff clone, the panic then no longer manifests when executing the final command.

---

_Review requested from @carljm by @AlexWaygood on 2025-10-06 10:03_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-06 10:03_

---

_Label `bug` added by @AlexWaygood on 2025-10-06 10:03_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-06 10:03_

---

_Label `ty` added by @AlexWaygood on 2025-10-06 10:03_

---

_Comment by @github-actions[bot] on 2025-10-06 10:05_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-06 10:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-10-06 10:08_

We _could_ consider just copying mypy completely here, and making all these modules non-shadowable: https://github.com/python/mypy/blob/3807423e9d98e678bf16b13ec8b4f909fe181908/mypy/build.py#L104-L117.

The underlying issue here is really one of documentation, though: the docs for the `extra-paths` option should mention the `python` option; possibly we should add aliases for the `python` option to make it clear that you should use it to point to your virtual environment; possibly we should rename `extra-paths` to something like `non-environment-paths` so that it sounds less "inviting" to users. Given that I think we'll need to make at least some of those changes anyway, I think it probably makes sense to add modules to the non-shadowable denylist as and when issues like this come up, rather than overly hastily assuming that they'll all cause issues for us just because they cause issues for (some) other type checkers.

---

_Review request for @sharkdp removed by @sharkdp on 2025-10-06 10:14_

---

_@MichaReiser approved on 2025-10-06 11:34_

---

_@AlexWaygood reviewed on 2025-10-06 11:39_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:673 on 2025-10-06 11:39_

```suggestion
/// unable to resolve builtin symbols. This is similar behaviour to other type checkers such
```

---

_Merged by @AlexWaygood on 2025-10-06 11:43_

---

_Closed by @AlexWaygood on 2025-10-06 11:43_

---

_Branch deleted on 2025-10-06 11:43_

---
