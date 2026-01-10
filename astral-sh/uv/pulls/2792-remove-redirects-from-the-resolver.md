```yaml
number: 2792
title: Remove redirects from the resolver
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/redirects
created_at: 2024-04-03T02:21:40Z
updated_at: 2024-04-03T02:43:58Z
url: https://github.com/astral-sh/uv/pull/2792
synced_at: 2026-01-10T14:49:08Z
```

# Remove redirects from the resolver

---

_Pull request opened by @charliermarsh on 2024-04-03 02:21_

## Summary

Rather than storing the `redirects` on the resolver, this PR just re-uses the "convert this URL to precise" logic when we convert to a `Resolution` after-the-fact. I think this is a lot simpler: it removes state from the resolver, and simplifies a lot of the hooks around distribution fetching (e.g., `get_or_build_wheel_metadata` no longer returns `(Metadata23, Option<Url>)`).


---

_Label `internal` added by @charliermarsh on 2024-04-03 02:21_

---

_Marked ready for review by @charliermarsh on 2024-04-03 02:21_

---

_@zanieb approved on 2024-04-03 02:24_

---

_Merged by @charliermarsh on 2024-04-03 02:43_

---

_Closed by @charliermarsh on 2024-04-03 02:43_

---

_Branch deleted on 2024-04-03 02:43_

---
