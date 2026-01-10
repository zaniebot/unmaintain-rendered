```yaml
number: 572
title: "Add a `--rebuild` flag to force rebuild of source distributions"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2023-12-05T18:22:26Z
updated_at: 2023-12-08T19:58:44Z
url: https://github.com/astral-sh/uv/issues/572
synced_at: 2026-01-10T05:40:31Z
```

# Add a `--rebuild` flag to force rebuild of source distributions

---

_Issue opened by @charliermarsh on 2023-12-05 18:22_

For URL- and path-based source distributions, we're going to assume they are always up-to-date once built. This greatly simplifies the caching and at least leads to predictable behavior. (In the future, we will likely implement smarter behavior around invalidation.) However, we need to provide users with a way to force a rebuild of specific packages (or all packages) that match these descriptions. This proposes uses a `--rebuild` flag which could optionally accept the names of packages. When we run with `--rebuild`, we'll (1) purge the cache of any built wheels for those packages, and then (2) mark any installed versions as extraneous in the install plan.

---

_Label `enhancement` added by @charliermarsh on 2023-12-05 18:22_

---

_Added to milestone `Feature complete` by @charliermarsh on 2023-12-05 18:22_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-07 05:28_

---

_Comment by @charliermarsh on 2023-12-07 05:42_

I'm gonna start by making `clean` take a list of packages, so we can implement the "cache purge" API.

---

_Closed by @charliermarsh on 2023-12-08 19:58_

---
