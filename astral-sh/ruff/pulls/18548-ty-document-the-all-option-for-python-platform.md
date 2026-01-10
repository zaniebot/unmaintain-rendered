```yaml
number: 18548
title: "[ty] document the `\"all\"` option for `python-platform`"
type: pull_request
state: merged
author: DetachHead
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: document-python-platform-all
created_at: 2025-06-08T09:31:40Z
updated_at: 2025-06-09T12:01:25Z
url: https://github.com/astral-sh/ruff/pull/18548
synced_at: 2026-01-10T18:45:04Z
```

# [ty] document the `"all"` option for `python-platform`

---

_Pull request opened by @DetachHead on 2025-06-08 09:31_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
fixes https://github.com/astral-sh/ty/issues/609
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
n/a
<!-- How was it tested? -->


---

_Review requested from @carljm by @DetachHead on 2025-06-08 09:31_

---

_Review requested from @MichaReiser by @DetachHead on 2025-06-08 09:31_

---

_Review requested from @AlexWaygood by @DetachHead on 2025-06-08 09:31_

---

_Review requested from @sharkdp by @DetachHead on 2025-06-08 09:31_

---

_Review requested from @dcreager by @DetachHead on 2025-06-08 09:31_

---

_Label `documentation` added by @AlexWaygood on 2025-06-08 09:32_

---

_Label `ty` added by @AlexWaygood on 2025-06-08 09:32_

---

_Comment by @github-actions[bot] on 2025-06-08 09:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@AlexWaygood reviewed on 2025-06-08 09:36_

---

_Review comment by @AlexWaygood on `crates/ty/docs/configuration.md`:1 on 2025-06-08 09:36_

Thank you! Unfortunately this file is generated — you'll need to update `options.rs` and then run `cargo dev generate-all` to update it

https://github.com/astral-sh/ruff/blob/86e5a311f05943095f8e7480dea1a69fdc8c787e/crates/ty/docs/configuration.md?plain=1#L1

---

_@DetachHead reviewed on 2025-06-08 09:47_

---

_Review comment by @DetachHead on `crates/ty/docs/configuration.md`:1 on 2025-06-08 09:47_

oops

---

_@AlexWaygood reviewed on 2025-06-08 11:03_

---

_Review comment by @AlexWaygood on `crates/ty_project/src/metadata/options.rs`:343 on 2025-06-08 11:03_

This bulleted list describes the value ty will fall back to if no platform is specified. Currently we will never fall back to `all`, so I don't think it's correct to add `all` to this bulleted list. Instead, I think we should add a mention of `all` to the paragraph preceding this bulleted list

---

_@AlexWaygood approved on 2025-06-09 11:58_

Thanks!

---

_Merged by @AlexWaygood on 2025-06-09 12:01_

---

_Closed by @AlexWaygood on 2025-06-09 12:01_

---
