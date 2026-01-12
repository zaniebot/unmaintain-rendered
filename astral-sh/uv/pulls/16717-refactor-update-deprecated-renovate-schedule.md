```yaml
number: 16717
title: "refactor: update deprecated renovate schedule syntax"
type: pull_request
state: merged
author: CatBraaain
labels:
  - internal
assignees: []
merged: true
base: main
head: update-deprecated-renovate-cron
created_at: 2025-11-13T05:34:42Z
updated_at: 2025-11-13T09:52:20Z
url: https://github.com/astral-sh/uv/pull/16717
synced_at: 2026-01-12T16:12:24Z
```

# refactor: update deprecated renovate schedule syntax

---

_@CatBraaain_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

Related issue: none
This is a minor change; no issue is associated. Please let me know if one is required.

## Summary
Update deprecated renovate schedule syntax
```diff
- schedule: ["before 4am on Monday"],
+ schedule: ["* 0-3 * * 1"],
```

<!-- What's the purpose of the change? What does it do, and why? -->
## Rationale
- Deprecated: Renovate plans to remove the @breejs/later library, so cron syntax is recommended.
- Time range: at least 3-4 hours recommended for any schedule.

References:
- [Deprecated later syntax](https://docs.renovatebot.com/key-concepts/scheduling/#deprecated-breejslater-syntax)
- [Schedule options](https://docs.renovatebot.com/configuration-options/#schedule)


## Test Plan
<!-- How was it tested? -->
- Cron syntax verified on [crontab.guru](https://crontab.guru/#*_0-3_*_*_1)
- Behavior in Renovate not testable directly


---

_Comment by @konstin on 2025-11-13 09:52_

crontab.guru has a different interpretation (we'd want something like `0 4 * * 1` in its terms), but the renovate docs indicate that this is the right new syntax.

---

_Label `internal` added by @konstin on 2025-11-13 09:52_

---

_@konstin approved on 2025-11-13 09:52_

Thanks!

---

_Merged by @konstin on 2025-11-13 09:52_

---

_Closed by @konstin on 2025-11-13 09:52_

---
