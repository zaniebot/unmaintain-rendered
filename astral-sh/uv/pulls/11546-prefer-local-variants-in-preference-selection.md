```yaml
number: 11546
title: Prefer local variants in preference selection
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/local-pref
created_at: 2025-02-16T01:14:09Z
updated_at: 2025-02-16T01:35:49Z
url: https://github.com/astral-sh/uv/pull/11546
synced_at: 2026-01-12T16:09:53Z
```

# Prefer local variants in preference selection

---

_@charliermarsh_

## Summary

This PR fixes a subtle issue arising from our propagation of preferences. When we resolve a fork, we take the solution from that fork and mark all the chosen versions as "preferred" as we move on to the next fork.

In this specific case, the resolver ended up solving a macOS-specific fork first, which led us to pick `2.6.0` rather than `2.6.0+cpu`. This in itself is correct; but when we moved on to the next fork, we preferred `2.6.0` over `2.6.0+cpu`, despite the fact that `2.6.0` _only_ includes macOS wheel, and that branch was focused on Linux.

Now, in preferences, we prefer local variants (if they exist). If the local variant ends up not working, we'll presumedly backtrack to the base version anyway.

Closes https://github.com/astral-sh/uv/issues/11406.


---

_Label `bug` added by @charliermarsh on 2025-02-16 01:14_

---

_Merged by @charliermarsh on 2025-02-16 01:35_

---

_Closed by @charliermarsh on 2025-02-16 01:35_

---

_Branch deleted on 2025-02-16 01:35_

---
