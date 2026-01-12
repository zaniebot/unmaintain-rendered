```yaml
number: 18827
title: "[ty] Support `--python=<symlink to executable>`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/python-symlink
created_at: 2025-06-20T17:36:20Z
updated_at: 2025-06-21T19:28:49Z
url: https://github.com/astral-sh/ruff/pull/18827
synced_at: 2026-01-12T15:56:26Z
```

# [ty] Support `--python=<symlink to executable>`

---

_@AlexWaygood_

## Summary

Fixes https://github.com/astral-sh/ty/issues/640. If a user passes `--python=<some-virtual-environment>/bin/python`, we must avoid canonicalizing the path until we've traversed upwards to find the `sys.prefix` directory (`<some-virtual-environment>`). On Unix systems, `<sys.prefix>/bin/python` is often a symlink to a system interpreter; if we resolve the symlink too easily then we'll add the system interpreter's `site-packages` directory as a search path rather than the virtual environment's directory.

## Test Plan

I added an integration test to `crates/ty/tests/cli/python_environment.rs` which fails on `main`. I also manually tested locally that running `cargo run -p ty check foo.py --python=.venv/bin/python -vv` now prints this log to the terminal

```
2025-06-20 18:35:24.57702 DEBUG Resolved site-packages directories for this virtual environment are: SitePackagesPaths({"/Users/alexw/dev/ruff/.venv/lib/python3.13/site-packages"})
```

Whereas it previously resolved `site-packages` to my system intallation's `site-packages` directory

---

_Label `ty` added by @AlexWaygood on 2025-06-20 17:36_

---

_Review requested from @carljm by @AlexWaygood on 2025-06-20 17:36_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-20 17:36_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-20 17:36_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-06-20 17:36_

---

_Label `bug` added by @AlexWaygood on 2025-06-20 17:38_

---

_Comment by @AlexWaygood on 2025-06-20 17:39_

(I think this used to work fine, but was broken by me in #18457, FWIW)

---

_Comment by @github-actions[bot] on 2025-06-20 17:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review request for @carljm removed by @carljm on 2025-06-20 21:31_

---

_@MichaReiser approved on 2025-06-21 17:31_

---

_Merged by @AlexWaygood on 2025-06-21 19:28_

---

_Closed by @AlexWaygood on 2025-06-21 19:28_

---

_Branch deleted on 2025-06-21 19:28_

---
