```yaml
number: 18988
title: "[`Airflow`] Make `AIR302` example error out-of-the-box"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-4
created_at: 2025-06-27T17:37:50Z
updated_at: 2025-07-02T19:59:15Z
url: https://github.com/astral-sh/ruff/pull/18988
synced_at: 2026-01-10T18:33:12Z
```

# [`Airflow`] Make `AIR302` example error out-of-the-box

---

_Pull request opened by @MeGaGiGaGon on 2025-06-27 17:37_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #18972

This PR makes [airflow3-moved-to-provider (AIR302)](https://docs.astral.sh/ruff/rules/airflow3-moved-to-provider/#airflow3-moved-to-provider-air302)'s example error out-of-the-box

[Old example](https://play.ruff.rs/1026c008-57bc-4330-93b9-141444f2a611)
```py
from airflow.auth.managers.fab.fab_auth_manage import FabAuthManager
```

[New example](https://play.ruff.rs/b690e809-a81d-4265-9fde-1494caa0b7fd)
```py
from airflow.auth.managers.fab.fab_auth_manager import FabAuthManager

fab_auth_manager_app = FabAuthManager().get_fastapi_app()
```

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Review requested from @ntBre by @ntBre on 2025-06-27 17:38_

---

_Label `documentation` added by @ntBre on 2025-06-27 17:38_

---

_Comment by @github-actions[bot] on 2025-06-27 18:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-06-30 13:44_

---

_Merged by @ntBre on 2025-06-30 13:45_

---

_Closed by @ntBre on 2025-06-30 13:45_

---

_Branch deleted on 2025-07-02 19:59_

---
