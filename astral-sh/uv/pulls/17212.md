```yaml
number: 17212
title: "fix: uv tree orphaned roots and premature deduplication"
type: pull_request
state: open
author: zelosleone
labels:
  - bug
assignees: []
base: main
head: fix-issue-#17160
created_at: 2025-12-21T22:53:47Z
updated_at: 2026-01-06T19:04:17Z
url: https://github.com/astral-sh/uv/pull/17212
synced_at: 2026-01-10T05:49:14Z
```

# fix: uv tree orphaned roots and premature deduplication

---

_Pull request opened by @zelosleone on 2025-12-21 22:53_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

will close https://github.com/astral-sh/uv/issues/17160

Basically, old code using nodes with no incoming edges included transitive deps which resulted in orphaned roots. We didnt actually need that code as well, infinite cycle handling was done in `fn visit` correctly so just using root node directly solves the issue. I also found another bug during the process where packages were marked as "visited" prematurely resulting in not even expanding them and not showing them at the tree.

## Test Plan

I added two tests with snapshots.


---

_Converted to draft by @zelosleone on 2025-12-21 22:59_

---

_Marked ready for review by @zelosleone on 2025-12-21 23:25_

---

_Review requested from @charliermarsh by @konstin on 2026-01-06 19:04_

---

_Label `bug` added by @konstin on 2026-01-06 19:04_

---
