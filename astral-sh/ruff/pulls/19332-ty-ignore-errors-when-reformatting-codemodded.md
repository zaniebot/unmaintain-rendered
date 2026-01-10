```yaml
number: 19332
title: "[ty] ignore errors when reformatting codemodded typeshed"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/fix-workflow
created_at: 2025-07-14T16:11:03Z
updated_at: 2025-07-14T16:14:03Z
url: https://github.com/astral-sh/ruff/pull/19332
synced_at: 2026-01-10T18:33:12Z
```

# [ty] ignore errors when reformatting codemodded typeshed

---

_Pull request opened by @AlexWaygood on 2025-07-14 16:11_

## Summary

Followup to https://github.com/astral-sh/ruff/pull/19311. Pre-commit exits with code 1 when any files are reformatted by black. But it's expected that some files will need to be reformatted here; this shouldn't cause the whole typeshed-sync job to fail (see https://github.com/astral-sh/ruff/actions/runs/16271785746).

## Test Plan

I'll re-run the workflow manually after merging this...


---

_Label `ci` added by @AlexWaygood on 2025-07-14 16:11_

---

_Label `ty` added by @AlexWaygood on 2025-07-14 16:11_

---

_Renamed from "ignore errors when reformatting codemodded typeshed" to "[ty] ignore errors when reformatting codemodded typeshed" by @AlexWaygood on 2025-07-14 16:11_

---

_Merged by @AlexWaygood on 2025-07-14 16:14_

---

_Closed by @AlexWaygood on 2025-07-14 16:14_

---

_Branch deleted on 2025-07-14 16:14_

---
