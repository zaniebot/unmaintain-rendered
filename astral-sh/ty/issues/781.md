```yaml
number: 781
title: "Don't run mypy_primer CI job on PRs that only touch Markdown files"
type: issue
state: closed
author: AlexWaygood
labels:
  - help wanted
  - testing
assignees: []
created_at: 2025-07-08T14:34:35Z
updated_at: 2025-07-09T18:40:45Z
url: https://github.com/astral-sh/ty/issues/781
synced_at: 2026-01-10T02:07:36Z
```

# Don't run mypy_primer CI job on PRs that only touch Markdown files

---

_Issue opened by @AlexWaygood on 2025-07-08 14:34_

We can be confident that these are never going to change the primer output; it's a waste of resources. We make quite a few PRs like this because all our tests are written in Markdown, and we often create a PR of failing tests before making a followup PR that makes the tests pass.

---

_Label `help wanted` added by @AlexWaygood on 2025-07-08 14:34_

---

_Label `testing` added by @AlexWaygood on 2025-07-08 14:34_

---

_Closed by @AlexWaygood on 2025-07-09 18:40_

---
