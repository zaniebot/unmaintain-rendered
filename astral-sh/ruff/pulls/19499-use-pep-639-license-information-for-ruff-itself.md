```yaml
number: 19499
title: Use PEP 639 license information for Ruff itself instead of classifier
type: pull_request
state: merged
author: DimitriPapadopoulos
labels: []
assignees: []
merged: true
base: main
head: PEP639
created_at: 2025-07-22T21:04:01Z
updated_at: 2025-07-31T09:11:02Z
url: https://github.com/astral-sh/ruff/pull/19499
synced_at: 2026-01-10T17:52:17Z
```

# Use PEP 639 license information for Ruff itself instead of classifier

---

_Pull request opened by @DimitriPapadopoulos on 2025-07-22 21:04_

## Summary

Declare licenses using only these two fields, as per PEP 639:
* `license`:       SPDX license expression consisting of one or more license identifiers
* `license-files`: list of license file glob patterns

Supported by maturin ≥ 1.9.0:
https://www.maturin.rs/changelog.html

## Test Plan

N/A

---

_Comment by @MichaReiser on 2025-07-28 07:48_

I removed the documentation label so that it appears under the "Other" section (we tend to remove the *Documentation* section in releases)

---

_Comment by @vivodi on 2025-07-31 07:56_

I see this PR has been fully reverted:
  - https://github.com/astral-sh/ruff/pull/19599
  - https://github.com/astral-sh/ruff/pull/19624

Will you @DimitriPapadopoulos have time to look into it?

---

_Comment by @DimitriPapadopoulos on 2025-07-31 09:11_

Unfortunately #19624 lacks context. Is the problem that `uv build` does not support PEP 639 yet?

---
