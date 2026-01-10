```yaml
number: 11473
title: Update prev token end before token bump
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/bump-bug
created_at: 2024-05-20T06:38:46Z
updated_at: 2024-05-20T06:39:02Z
url: https://github.com/astral-sh/ruff/pull/11473
synced_at: 2026-01-10T22:05:26Z
```

# Update prev token end before token bump

---

_Pull request opened by @dhruvmanila on 2024-05-20 06:38_

## Summary

This PR fixes a bug where the parser would bump the token before updating the `prev_token_end`. The order should be reversed.


---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-20 06:38_

---

_Renamed from "Make `Lexer` lazy (#11244)" to "Update prev token end before token bump" by @dhruvmanila on 2024-05-20 06:38_

---

_Merged by @dhruvmanila on 2024-05-20 06:39_

---

_Closed by @dhruvmanila on 2024-05-20 06:39_

---

_Branch deleted on 2024-05-20 06:39_

---
