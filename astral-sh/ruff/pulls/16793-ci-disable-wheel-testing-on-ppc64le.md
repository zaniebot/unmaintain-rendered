```yaml
number: 16793
title: "[ci]: Disable wheel testing on `ppc64le`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: micha/release-disable-ppc64le-testing
created_at: 2025-03-17T08:43:29Z
updated_at: 2025-03-17T12:35:46Z
url: https://github.com/astral-sh/ruff/pull/16793
synced_at: 2026-01-12T15:55:57Z
```

# [ci]: Disable wheel testing on `ppc64le`

---

_@MichaReiser_

## Summary

The PPC64le wheel testing job spuriously failes due to some race when installing python dependencies. 
This is very annoying because it requires restarting the release process over and over again until you're lucky and it passes. 

This PR disables wheel testing on PPC64le

This is the same as we did in uv, see https://github.com/astral-sh/uv/issues/11231

## Test Plan

The wheel test step was skipped in CI, see https://github.com/astral-sh/ruff/actions/runs/13895143309/job/38874065160?pr=16793 but it still runs for other targets


---

_Label `ci` added by @MichaReiser on 2025-03-17 08:43_

---

_Review requested from @ntBre by @MichaReiser on 2025-03-17 08:45_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-03-17 08:45_

---

_Comment by @github-actions[bot] on 2025-03-17 08:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "[ci]: Disable wheel testing on ppc64le]" to "[ci]: Disable wheel testing on `ppc64le`" by @MichaReiser on 2025-03-17 08:57_

---

_@ntBre approved on 2025-03-17 12:32_

Thanks! I think I ran into the same issue with one of the aarch64 targets, but this one seems the flakiest.

---

_Merged by @MichaReiser on 2025-03-17 12:35_

---

_Closed by @MichaReiser on 2025-03-17 12:35_

---

_Branch deleted on 2025-03-17 12:35_

---
