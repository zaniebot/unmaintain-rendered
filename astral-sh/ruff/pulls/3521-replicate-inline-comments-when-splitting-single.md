```yaml
number: 3521
title: Replicate inline comments when splitting single-line imports
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/comments
created_at: 2023-03-14T18:36:34Z
updated_at: 2023-03-14T18:48:15Z
url: https://github.com/astral-sh/ruff/pull/3521
synced_at: 2026-01-12T04:39:45Z
```

# Replicate inline comments when splitting single-line imports

---

_Pull request opened by @charliermarsh on 2023-03-14 18:36_

## Summary

If a user has an inline comment, like `from foo import bar, baz  # type: ignore`, and they use the "force-split-imports" mode, then today, we end up attaching that comment to the first split import (but no others). This PR modifies the formatting such that we replicate the comment across each split import, which better matches user intent (since end-of-line comments on imports tend to be action directives).

Closes #3418.


---

_Label `bug` added by @charliermarsh on 2023-03-14 18:36_

---

_Merged by @charliermarsh on 2023-03-14 18:48_

---

_Closed by @charliermarsh on 2023-03-14 18:48_

---

_Branch deleted on 2023-03-14 18:48_

---

_Comment by @github-actions[bot] on 2023-03-14 18:48_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---
