```yaml
number: 17039
title: "Refactor the Changelog for use in `report_dry_run` "
type: pull_request
state: merged
author: EliteTK
labels:
  - internal
assignees: []
merged: true
base: main
head: tk/dry-run-refactor-2
created_at: 2025-12-08T22:11:56Z
updated_at: 2025-12-12T10:37:32Z
url: https://github.com/astral-sh/uv/pull/17039
synced_at: 2026-01-10T05:49:14Z
```

# Refactor the Changelog for use in `report_dry_run` 

---

_Pull request opened by @EliteTK on 2025-12-08 22:11_

## Summary

Remove duplication in `report_dry_run` by making `Changelog` support both local and remote dists. This is in support of #16653 and will form a new basis for #16981.

This also involved refactoring `InstallLogger` and its implementations to support dry run logging.

Additionally includes some minor refactoring in `SummaryInstallLogger` and a fix to `InstalledVersion`.

See https://github.com/astral-sh/uv/compare/tk/dry-run-refactor for an alternative approach (although obviously comes with some caveats).

## Test Plan

There are already quite a few tests which cover the output and they pass. Manual testing was used to ensure styling stayed consistent.

---

_Review requested from @zanieb by @EliteTK on 2025-12-08 22:11_

---

_Review requested from @konstin by @EliteTK on 2025-12-08 22:11_

---

_Label `internal` added by @EliteTK on 2025-12-08 22:11_

---

_@zanieb approved on 2025-12-09 21:19_

---

_Merged by @EliteTK on 2025-12-12 10:37_

---

_Closed by @EliteTK on 2025-12-12 10:37_

---

_Branch deleted on 2025-12-12 10:37_

---
