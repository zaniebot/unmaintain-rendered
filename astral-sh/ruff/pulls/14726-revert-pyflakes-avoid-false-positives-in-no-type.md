```yaml
number: 14726
title: "Revert: [pyflakes] Avoid false positives in `@no_type_check` contexts (F821, F722) (#14615)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
assignees: []
merged: true
base: main
head: micha/revert-no-type-check
created_at: 2024-12-02T12:19:06Z
updated_at: 2024-12-02T13:28:29Z
url: https://github.com/astral-sh/ruff/pull/14726
synced_at: 2026-01-10T20:42:27Z
```

# Revert: [pyflakes] Avoid false positives in `@no_type_check` contexts (F821, F722) (#14615)

---

_Pull request opened by @MichaReiser on 2024-12-02 12:19_

## Summary

Reverts #14615 due to https://github.com/astral-sh/ruff/issues/14698

The fix-bug seems worse than avoiding the few false positives. That's why I think we should revert this change
for now to give us more time to work on proper `@no_type_check` support.

Fixes https://github.com/astral-sh/ruff/issues/14698


---

_Label `bug` added by @MichaReiser on 2024-12-02 12:19_

---

_Marked ready for review by @MichaReiser on 2024-12-02 12:33_

---

_@charliermarsh approved on 2024-12-02 13:26_

Thanks. The PR itself was solid but it was my mistake to take this approach.

---

_@AlexWaygood approved on 2024-12-02 13:26_

---

_Merged by @MichaReiser on 2024-12-02 13:28_

---

_Closed by @MichaReiser on 2024-12-02 13:28_

---

_Branch deleted on 2024-12-02 13:28_

---
