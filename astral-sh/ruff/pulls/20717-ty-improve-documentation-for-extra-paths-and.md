```yaml
number: 20717
title: "[ty] Improve documentation for `extra-paths` and `python` config settings"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: alex/extra-paths-docs
created_at: 2025-10-06T11:06:04Z
updated_at: 2025-10-06T12:20:02Z
url: https://github.com/astral-sh/ruff/pull/20717
synced_at: 2026-01-12T15:57:08Z
```

# [ty] Improve documentation for `extra-paths` and `python` config settings

---

_@AlexWaygood_

## Summary

This PR attempts to improve our documentation to steer users away from the `extra-paths` option and towards the `python` option. `extra-paths` is really an advanced configuration option that most users shouldn't need.

I actually ended up making the `--help` text for `--python` shorter (it was too verbose for a `--help` command on the terminal, in my opinion), instead moving some of the information there into the docs for the pyproject.toml `python` option.

Helps with https://github.com/astral-sh/ty/issues/1289

---

_Review requested from @carljm by @AlexWaygood on 2025-10-06 11:06_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-10-06 11:06_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-06 11:06_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-06 11:06_

---

_Label `documentation` added by @AlexWaygood on 2025-10-06 11:06_

---

_Label `ty` added by @AlexWaygood on 2025-10-06 11:06_

---

_Comment by @github-actions[bot] on 2025-10-06 11:07_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-06 11:09_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @MichaReiser on `crates/ty/src/args.rs`:56 on 2025-10-06 11:57_

```suggestion
    /// ty uses your Python environment to resolve third-party imports in your code.
```

I believe using Ty is incorrect according to our branding guide  (CC: @zanieb to confirm)

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:586 on 2025-10-06 11:58_

```suggestion
    /// ty uses the `site-packages` directory of your project's Python environment
```

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:591 on 2025-10-06 11:58_

```suggestion
    /// environment variable to point to your project's virtual environment. ty can also infer
```

---

_@MichaReiser approved on 2025-10-06 11:58_

---

_@MichaReiser reviewed on 2025-10-06 12:13_

---

_Review comment by @MichaReiser on `crates/ty/src/args.rs`:56 on 2025-10-06 12:13_

For reference https://github.com/astral-sh/uv/blob/main/STYLE.md#styling-uv

---

_Merged by @AlexWaygood on 2025-10-06 12:20_

---

_Closed by @AlexWaygood on 2025-10-06 12:20_

---

_Branch deleted on 2025-10-06 12:20_

---
