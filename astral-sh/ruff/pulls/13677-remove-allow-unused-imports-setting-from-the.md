```yaml
number: 13677
title: "Remove `allow-unused-imports` setting from the common lint options"
type: pull_request
state: merged
author: MichaReiser
labels:
  - configuration
assignees: []
merged: true
base: ruff-0.7
head: micha/remove-allow-unused-imports-from-lint-options
created_at: 2024-10-08T09:46:17Z
updated_at: 2024-10-08T14:48:33Z
url: https://github.com/astral-sh/ruff/pull/13677
synced_at: 2026-01-12T15:55:45Z
```

# Remove `allow-unused-imports` setting from the common lint options

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/13668

Luckily, the `lint.allow_unused_imports` setting isn't read anywhere.
However, this could still be considered a breaking change because Ruff might fail to read a previously valid configuration. 
I do think that it should be very unlikely, considering that the configuration was "useless" before.

The setting remains available under `lint.pyflakes`

https://github.com/astral-sh/ruff/blob/0035ca9fe7cb1c016ec2811f100a95796c0f455e/crates/ruff_workspace/src/options.rs#L2824-L2829

## Test Plan

`cargo test`
<!-- How was it tested? -->


---

_Label `configuration` added by @MichaReiser on 2024-10-08 09:46_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-10-08 10:41_

---

_Comment by @github-actions[bot] on 2024-10-08 10:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Added to milestone `v0.7` by @MichaReiser on 2024-10-08 13:47_

---

_@AlexWaygood approved on 2024-10-08 13:49_

LGTM as part of a minor release (which it will be!)

---

_@zanieb approved on 2024-10-08 14:32_

---

_Merged by @MichaReiser on 2024-10-08 14:48_

---

_Closed by @MichaReiser on 2024-10-08 14:48_

---

_Branch deleted on 2024-10-08 14:48_

---
