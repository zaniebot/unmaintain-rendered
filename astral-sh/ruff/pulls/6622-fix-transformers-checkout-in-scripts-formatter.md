```yaml
number: 6622
title: Fix transformers checkout in scripts/formatter_ecosystem_checks.sh
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/transformers
created_at: 2023-08-16T16:40:47Z
updated_at: 2023-08-16T17:25:47Z
url: https://github.com/astral-sh/ruff/pull/6622
synced_at: 2026-01-12T15:55:22Z
```

# Fix transformers checkout in scripts/formatter_ecosystem_checks.sh

---

_@charliermarsh_

## Summary

In #6387, we accidentally added `git -C "$dir/django" checkout 95e4d6b81312fdd9f8ebf3385be1c1331168b5cf` as the transformers checkout (duplicated line from the Django case). This PR fixes the SHA, and spaces out the cases to make it more visible. I _think_ the net effect here is that we've been formatting `main` on transformers, rather than the SHA?


---

_Label `internal` added by @charliermarsh on 2023-08-16 16:40_

---

_Marked ready for review by @charliermarsh on 2023-08-16 16:40_

---

_@MichaReiser approved on 2023-08-16 16:44_

---

_Comment by @github-actions[bot] on 2023-08-16 16:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @zanieb on 2023-08-16 17:25_

---

_Closed by @zanieb on 2023-08-16 17:25_

---

_Branch deleted on 2023-08-16 17:25_

---
