```yaml
number: 3908
title: Create distinct graph nodes for each package extra
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/ex
created_at: 2024-05-29T15:27:34Z
updated_at: 2024-05-29T15:42:50Z
url: https://github.com/astral-sh/uv/pull/3908
synced_at: 2026-01-12T16:05:55Z
```

# Create distinct graph nodes for each package extra

---

_@charliermarsh_

## Summary

Today, we represent each package as a single node in the graph, and combine all the extras. This is helpful for the `requirements.txt`-style resolution, in which we want to show each a single line for each package with the extras combined into a single array.

This PR modifies the representation to instead use a separate node for each (package, extra) pair. We then reduce into the previous format when printing in the `requirements.txt`-style format, so there shouldn't be any user-facing changes here.

---

_Label `internal` added by @charliermarsh on 2024-05-29 15:29_

---

_Marked ready for review by @charliermarsh on 2024-05-29 15:29_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution/display.rs`:87 on 2024-05-29 15:30_

There might be a cleaner way to do this?

---

_@charliermarsh reviewed on 2024-05-29 15:30_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-29 15:30_

---

_@BurntSushi approved on 2024-05-29 15:38_

---

_Merged by @charliermarsh on 2024-05-29 15:42_

---

_Closed by @charliermarsh on 2024-05-29 15:42_

---

_Branch deleted on 2024-05-29 15:42_

---
