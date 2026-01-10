```yaml
number: 1057
title: "Add `--refresh` behavior to the cache"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/cache-refresh
created_at: 2024-01-23T05:23:20Z
updated_at: 2024-01-23T23:30:27Z
url: https://github.com/astral-sh/uv/pull/1057
synced_at: 2026-01-10T15:39:03Z
```

# Add `--refresh` behavior to the cache

---

_Pull request opened by @charliermarsh on 2024-01-23 05:23_

## Summary

This PR is an alternative approach to #949 which should be much safer. As in #949, we add a `Refresh` policy to the cache. However, instead of deleting entries from the cache the first time we read them, we now check if the entry is sufficiently new (created after the start of the command) if the refresh policy applies. If the entry is stale, then we avoid reading it and continue onward, relying on the cache to appropriately overwrite based on "new" data. (This relies on the preceding PRs, which ensure the cache is append-only, and ensure that we can atomically overwrite.)

Unfortunately, there are just a lot of paths through the cache, and didn't data is handled with different policies, so I really had to go through and consider the "right" behavior for each case. For example, the HTTP requests can use `max-age=0, must-revalidate`. But for the routes that are based on filesystem modification, we need to do something slightly different.

Closes #945.


---

_Marked ready for review by @charliermarsh on 2024-01-23 05:33_

---

_@charliermarsh reviewed on 2024-01-23 05:34_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/source/mod.rs`:973 on 2024-01-23 05:34_

If you're installing from an archive, like `flask @ ./flask.tar.gz`, then we trust the modification timestamp and ignore freshness requirements. (If you're installing from a directory, and so we're using the timestamp of the `pyproject.toml`, we _don't_ trust the timestamp, and respect freshness requirements.)

---

_@charliermarsh reviewed on 2024-01-23 05:51_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/distribution_database.rs`:123 on 2024-01-23 05:51_

I plan to do at least the first part of this in a follow-up PR. (It's easy and covers the vast majority of common cases, since we almost always have hashes.)

---

_@charliermarsh reviewed on 2024-01-23 05:51_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/distribution_database.rs`:196 on 2024-01-23 05:51_

I think this requires that we write the caching data _somewhere_, e.g., we'd need to have a file like `foo.whl.cache` next to the `foo.whl` directory.

---

_Review requested from @zanieb by @charliermarsh on 2024-01-23 05:53_

---

_Review requested from @konstin by @charliermarsh on 2024-01-23 05:53_

---

_Label `enhancement` added by @charliermarsh on 2024-01-23 05:53_

---

_@konstin approved on 2024-01-23 15:29_

---

_@zanieb approved on 2024-01-23 23:04_

LGTM :)

---

_Merged by @charliermarsh on 2024-01-23 23:30_

---

_Closed by @charliermarsh on 2024-01-23 23:30_

---

_Branch deleted on 2024-01-23 23:30_

---
