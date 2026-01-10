```yaml
number: 16344
title: Re-order changelog entries for 0.9.7
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/changelog
created_at: 2025-02-24T09:06:34Z
updated_at: 2025-02-24T09:10:16Z
url: https://github.com/astral-sh/ruff/pull/16344
synced_at: 2026-01-10T19:49:01Z
```

# Re-order changelog entries for 0.9.7

---

_Pull request opened by @dhruvmanila on 2025-02-24 09:06_

## Summary

This is mainly on me for not noticing this during the last release but I noticed in the last changelog that there's only 1 bug fix which didn't seem correct as I saw multiple of them so I looked at a couple of PRs that are in "Rule changes" section and the PRs that were marked with the `bug` label was categorized there because

1. It _also_ had other labels like `rule` and `fixes` (https://github.com/astral-sh/ruff/pull/16080, https://github.com/astral-sh/ruff/pull/16110, https://github.com/astral-sh/ruff/pull/16219, etc.)
2. Some PRs didn't have the `bug` label (but the issue as marked as `bug`) but _only_ labels like "fixes" (https://github.com/astral-sh/ruff/pull/16011, https://github.com/astral-sh/ruff/pull/16132, etc.)

---

_Label `internal` added by @dhruvmanila on 2025-02-24 09:06_

---

_Merged by @dhruvmanila on 2025-02-24 09:10_

---

_Closed by @dhruvmanila on 2025-02-24 09:10_

---

_Branch deleted on 2025-02-24 09:10_

---
