```yaml
number: 18549
title: "[ty] document how the default value for `python-version` is determined"
type: pull_request
state: merged
author: DetachHead
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: document-requires-python
created_at: 2025-06-08T10:05:31Z
updated_at: 2025-06-09T13:32:44Z
url: https://github.com/astral-sh/ruff/pull/18549
synced_at: 2026-01-10T18:45:04Z
```

# [ty] document how the default value for `python-version` is determined

---

_Pull request opened by @DetachHead on 2025-06-08 10:05_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
fixes https://github.com/astral-sh/ty/issues/608

i'm not sure what the best way to format this is
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
n/a
<!-- How was it tested? -->


---

_Review requested from @carljm by @DetachHead on 2025-06-08 10:05_

---

_Review requested from @MichaReiser by @DetachHead on 2025-06-08 10:05_

---

_Review requested from @AlexWaygood by @DetachHead on 2025-06-08 10:05_

---

_Review requested from @sharkdp by @DetachHead on 2025-06-08 10:05_

---

_Review requested from @dcreager by @DetachHead on 2025-06-08 10:05_

---

_@AlexWaygood reviewed on 2025-06-08 11:02_

---

_Review comment by @AlexWaygood on `crates/ty_project/src/metadata/options.rs`:325 on 2025-06-08 11:02_

Unfortunately it's a _bit_ more complicated... if we can't find a `project.requires-python` field in your pyproject.toml file, we'll then see if you have a virtual environment activated/specified/detected and try to figure out what Python version you're using from the virtual environment's metadata file. Only if that _also_ fails will we fall back to the 3.13 default. (See https://github.com/astral-sh/ty/tree/main/docs#python-version).

I think rather than trying to cram this into the `default` keyword here, it might be better to just leave the `default` here as `3.13` and add a paragraph to the doc-comment above this field that explains our full behaviour

---

_Label `documentation` added by @AlexWaygood on 2025-06-08 11:33_

---

_Label `ty` added by @AlexWaygood on 2025-06-08 11:33_

---

_Renamed from "[ty] document that `python-version` defaults to `project.requires-python` if present in `pyproject.toml`" to "[ty] document how the default value for `python-version` is determined" by @DetachHead on 2025-06-09 12:29_

---

_Comment by @github-actions[bot] on 2025-06-09 12:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@AlexWaygood approved on 2025-06-09 13:29_

Thank you!

---

_Merged by @AlexWaygood on 2025-06-09 13:32_

---

_Closed by @AlexWaygood on 2025-06-09 13:32_

---
