```yaml
number: 20361
title: "[ruff]: Build loongarch64 binaries in CI"
type: pull_request
state: merged
author: SkyBird233
labels:
  - ci
assignees: []
merged: true
base: main
head: feat/loongarch64
created_at: 2025-09-12T08:23:38Z
updated_at: 2025-09-12T17:49:14Z
url: https://github.com/astral-sh/ruff/pull/20361
synced_at: 2026-01-10T17:40:28Z
```

# [ruff]: Build loongarch64 binaries in CI

---

_Pull request opened by @SkyBird233 on 2025-09-12 08:23_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR adds support for building loongarch64 binaries in CI. As such support has been merged in uv (astral-sh/uv#15387) it's time to consider adding it to ruff.

Please note that as Ubuntu is not yet available for loongarch64, I have elected to use a Debian Trixie container maintained by community members. In addition, as Debian's pip does not allow installing modules system-wide, I have modified the workflow to install additional modules in a virtual environment.

Since the workflow is shared between all targets, the only way to handle this difference (between Debian and Ubuntu) is just to install pip in a venv for all targets. If there is a better (and less intrusive) way to work around this, please let me know.

## Test Plan

Tests are included in CI and the loongarch64 artifacts built in [this workflow](https://github.com/SkyBird233/ruff/actions/runs/17640270032/job/50125471548) has been smoke tested.


---

_Comment by @github-actions[bot] on 2025-09-12 13:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-09-12 17:48_

Thank you! This looks good to me

---

_Label `ci` added by @ntBre on 2025-09-12 17:49_

---

_Merged by @ntBre on 2025-09-12 17:49_

---

_Closed by @ntBre on 2025-09-12 17:49_

---
