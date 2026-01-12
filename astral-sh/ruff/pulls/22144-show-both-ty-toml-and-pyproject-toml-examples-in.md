```yaml
number: 22144
title: "Show both `ty.toml` and `pyproject.toml` examples in configuration reference"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: ty-toml-docs-tab
created_at: 2025-12-22T17:53:49Z
updated_at: 2025-12-27T00:21:25Z
url: https://github.com/astral-sh/ruff/pull/22144
synced_at: 2026-01-12T15:57:42Z
```

# Show both `ty.toml` and `pyproject.toml` examples in configuration reference

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves https://github.com/astral-sh/ty/issues/2166

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @carljm by @MatthewMckee4 on 2025-12-22 17:53_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-12-22 17:53_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-12-22 17:53_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-12-22 17:53_

---

_Converted to draft by @MatthewMckee4 on 2025-12-22 17:57_

---

_Label `documentation` added by @AlexWaygood on 2025-12-22 18:02_

---

_Label `ty` added by @AlexWaygood on 2025-12-22 18:02_

---

_Comment by @MatthewMckee4 on 2025-12-22 22:33_

Unfortunately the `## overrides` section has hardcoded [tool.ty....], So i cant really change this in a nice way. Unless i change the doc comment

---

_Marked ready for review by @MatthewMckee4 on 2025-12-22 22:34_

---

_Review request for @carljm removed by @carljm on 2025-12-23 01:48_

---

_Comment by @dhruvmanila on 2025-12-23 06:21_

> Unfortunately the `## overrides` section has hardcoded [tool.ty....], So i cant really change this in a nice way. Unless i change the doc comment

I think it's fine to keep that as-is for now.

It would be great if you can provide a screen capture of this change in the rendered documentation.

---

_Comment by @MatthewMckee4 on 2025-12-23 07:17_

Okay will do

---

_Closed by @MichaReiser on 2025-12-23 08:15_

---

_Reopened by @MichaReiser on 2025-12-23 08:15_

---

_Renamed from "Ty toml docs tab" to "Show both `ty.toml` and `pyproject.toml` examples in configuration reference" by @MichaReiser on 2025-12-23 08:16_

---

_Comment by @MichaReiser on 2025-12-23 08:20_

Here's a recording

https://github.com/user-attachments/assets/7a6c0239-3c00-4b90-be04-71e6b940f735



---

_@MichaReiser reviewed on 2025-12-23 08:31_

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_ty_options.rs`:231 on 2025-12-23 08:31_

I don't see any changes if I comment this out. Can you say more why this is needed.

---

_Merged by @MichaReiser on 2025-12-23 08:49_

---

_Closed by @MichaReiser on 2025-12-23 08:49_

---

_Branch deleted on 2025-12-27 00:21_

---
