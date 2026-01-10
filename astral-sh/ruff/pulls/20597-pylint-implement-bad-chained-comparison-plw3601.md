```yaml
number: 20597
title: "[`pylint`] Implement `bad-chained-comparison` (`PLW3601`)"
type: pull_request
state: closed
author: kapkekes
labels: []
assignees: []
base: main
head: w3601-bad-chained-comparison
created_at: 2025-09-26T21:25:46Z
updated_at: 2025-09-29T20:46:21Z
url: https://github.com/astral-sh/ruff/pull/20597
synced_at: 2026-01-10T17:40:28Z
```

# [`pylint`] Implement `bad-chained-comparison` (`PLW3601`)

---

_Pull request opened by @kapkekes on 2025-09-26 21:25_

## Summary

Part of #970.

Implements `PLW3601` to fully match the original `W3601`.

Closes #20531, probably?

## Test Plan

`cargo nextest run` and `cargo insta test`.

---

_Comment by @ntBre on 2025-09-29 20:46_

I'll go ahead and close this for now, based on my comment on the issue (https://github.com/astral-sh/ruff/issues/20531#issuecomment-3348988228) and the reviews from the earlier PR (https://github.com/astral-sh/ruff/pull/10028).

Thank you for working on this and sorry again for not catching the earlier work sooner!

---

_Closed by @ntBre on 2025-09-29 20:46_

---
