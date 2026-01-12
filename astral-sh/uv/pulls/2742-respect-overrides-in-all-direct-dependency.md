```yaml
number: 2742
title: Respect overrides in all direct-dependency iterators
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/constraints-ii
created_at: 2024-03-31T16:53:22Z
updated_at: 2024-03-31T18:03:49Z
url: https://github.com/astral-sh/uv/pull/2742
synced_at: 2026-01-12T16:05:12Z
```

# Respect overrides in all direct-dependency iterators

---

_@charliermarsh_

## Summary

We iterate over the project "requirements" directly in a variety of places. However, it's not always the case that an input "requirement" on its own will _actually_ be part of the resolution, since we support "overrides".

Historically, then, overrides haven't worked as expected for _direct_ dependencies (and we have some tests that demonstrate the current, "wrong" behavior). This is just a bug, but it's not really one that comes up in practice, since it's rare to apply an override to your _own_ dependency.

However, we're now considering expanding the lookahead concept to include local transitive dependencies. In this case, it's more and more important that overrides and constraints are handled consistently.

This PR modifies all the locations in which we iterate over requirements directly, and modifies them to respect overrides (and constraints, where necessary).


---

_Label `bug` added by @charliermarsh on 2024-03-31 16:53_

---

_Marked ready for review by @charliermarsh on 2024-03-31 16:53_

---

_@charliermarsh reviewed on 2024-03-31 16:53_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution.rs`:437 on 2024-03-31 16:53_

Editables were missing here.

---

_@charliermarsh reviewed on 2024-03-31 16:54_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/urls.rs`:134 on 2024-03-31 16:54_

Able to delete a ton of code here because overrides are now applied in the _iterator_, rather than needing a special case down here.

---

_Merged by @charliermarsh on 2024-03-31 18:03_

---

_Closed by @charliermarsh on 2024-03-31 18:03_

---

_Branch deleted on 2024-03-31 18:03_

---
