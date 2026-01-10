```yaml
number: 14042
title: "Switch to `uv publish`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - release
assignees: []
merged: true
base: main
head: dhruv/uv-publish
created_at: 2024-11-01T14:24:40Z
updated_at: 2024-11-01T14:54:31Z
url: https://github.com/astral-sh/ruff/pull/14042
synced_at: 2026-01-10T20:59:37Z
```

# Switch to `uv publish`

---

_Pull request opened by @dhruvmanila on 2024-11-01 14:24_

## Summary

Ref: https://github.com/astral-sh/uv/pull/8065

## Test Plan

Going to re-release `0.7.2` which failed: https://github.com/astral-sh/ruff/actions/runs/11630280069


---

_Label `release` added by @dhruvmanila on 2024-11-01 14:24_

---

_Comment by @dhruvmanila on 2024-11-01 14:25_

Not sure if there's a way to test this but I'm going to re-release `0.7.2` which failed using the existing workflow.

---

_Review requested from @konstin by @dhruvmanila on 2024-11-01 14:25_

---

_@AlexWaygood approved on 2024-11-01 14:26_

Nice!

---

_Comment by @AlexWaygood on 2024-11-01 14:27_

> Not sure if there's a way to test this but I'm going to re-release `0.7.2` which failed using the existing workflow.

I applied https://github.com/AlexWaygood/typeshed-stats/commit/8ce3edb042dbc7a6eb468e25de6dfee66a411d5b a few weeks ago, which is very similar to this PR. It's worked a treat for me, (and given me a big speedup for that workflow!)

---

_Merged by @dhruvmanila on 2024-11-01 14:54_

---

_Closed by @dhruvmanila on 2024-11-01 14:54_

---

_Branch deleted on 2024-11-01 14:54_

---
