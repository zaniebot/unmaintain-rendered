```yaml
number: 18207
title: "[ty] Add note to `unresolved-import` hinting to users to configure their Python environment"
type: pull_request
state: merged
author: EmilyBZhang
labels:
  - ty
assignees: []
merged: true
base: main
head: emily/unresolved-import-python-env-note
created_at: 2025-05-19T20:38:50Z
updated_at: 2025-05-19T21:31:26Z
url: https://github.com/astral-sh/ruff/pull/18207
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Add note to `unresolved-import` hinting to users to configure their Python environment

---

_Pull request opened by @EmilyBZhang on 2025-05-19 20:38_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

Closes https://github.com/astral-sh/ty/issues/453.

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Add an additional info diagnostic to `unresolved-import` check to hint to users that they should make sure their Python environment is properly configured for ty, linking them to the corresponding doc. This diagnostic is only shown when an import is not relative, e.g., `import maturin` not `import .maturin`.

## Test Plan

<!-- How was it tested? -->

Updated snapshots with new info message and reran tests.


---

_Review requested from @carljm by @EmilyBZhang on 2025-05-19 20:38_

---

_Review requested from @AlexWaygood by @EmilyBZhang on 2025-05-19 20:38_

---

_Review requested from @sharkdp by @EmilyBZhang on 2025-05-19 20:38_

---

_Review requested from @dcreager by @EmilyBZhang on 2025-05-19 20:38_

---

_Review requested from @MichaReiser by @EmilyBZhang on 2025-05-19 20:38_

---

_Label `ty` added by @AlexWaygood on 2025-05-19 21:09_

---

_@AlexWaygood approved on 2025-05-19 21:11_

This looks great to me! I guess my only concern is that the link could become stale in the future, since our docs aren't particularly stable right now. Will wait for another maintainer's approval before landing.

---

_Comment by @github-actions[bot] on 2025-05-19 21:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@dcreager approved on 2025-05-19 21:23_

This looks great, thank you!

I'm okay with the link to the GitHub repo readme — we will want to update that to a more stable URL in the future once we have the docs hosted in a more stable place. But I don't think that needs to block this, since we'll want to do a careful vet of links when we do that anyway!

---

_Merged by @dcreager on 2025-05-19 21:24_

---

_Closed by @dcreager on 2025-05-19 21:24_

---

_Branch deleted on 2025-05-19 21:31_

---
