```yaml
number: 2285
title: Allow more-precise Git URLs to override less-precise Git URLs
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/constraints
created_at: 2024-03-07T19:18:06Z
updated_at: 2024-03-07T23:47:32Z
url: https://github.com/astral-sh/uv/pull/2285
synced_at: 2026-01-10T14:54:43Z
```

# Allow more-precise Git URLs to override less-precise Git URLs

---

_Pull request opened by @charliermarsh on 2024-03-07 19:18_

## Summary

This PR removes the URL conflict errors when the output of a `uv pip compile` is used as a constraint to a subsequent `uv pip compile`.

If you run `uv pip compile`, the output file will contain your Git dependencies, but pinned to a specific commit, like:

```
git+https://github.com/pallets/werkzeug@32e69512134c2f8183c6438b2b2e13fd24e9d19f
```

If you then use the output as a constraint to a subsequent resolution (e.g., perhaps you require `git+https://github.com/pallets/werkzeug@main`), we currently fail. I think this is a reasonable workflow to support when all of these requirements are coming from _your own_ dependencies. So we now assume when resolving that the former is a more precise variant of the latter.

Closes https://github.com/astral-sh/uv/issues/1903.

Closes https://github.com/astral-sh/uv/issues/2266.

## Test Plan

`cargo test`


---

_Label `enhancement` added by @charliermarsh on 2024-03-07 19:18_

---

_Review requested from @zanieb by @charliermarsh on 2024-03-07 19:21_

---

_Comment by @sbidoul on 2024-03-07 20:59_

Nice! Supporting this workflow is one of the reasons I created pip-deepfreeze. So 👌

---

_@zanieb approved on 2024-03-07 23:37_

---

_Merged by @charliermarsh on 2024-03-07 23:47_

---

_Closed by @charliermarsh on 2024-03-07 23:47_

---

_Branch deleted on 2024-03-07 23:47_

---
