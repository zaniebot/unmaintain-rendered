```yaml
number: 6025
title: "Filter mixed sources from `--find-links` entries in lockfile"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
  - lock
assignees: []
merged: true
base: main
head: charlie/mixed-links
created_at: 2024-08-12T02:16:44Z
updated_at: 2024-08-12T12:54:12Z
url: https://github.com/astral-sh/uv/pull/6025
synced_at: 2026-01-12T16:07:09Z
```

# Filter mixed sources from `--find-links` entries in lockfile

---

_@charliermarsh_

## Summary

Our current handling of `--find-links` merges the entries in each index. As a result, we can end up with `AnnotatedDist` entries that reference distributions across indexes.

I'd like to change `--find-links` such that each `--find-links` entry is just treated as its own index (so, e.g., if `requests` exists in the first `--find-links` entry, we don't even check the registry by default), which would _also_ fix this problem automatically. But that's a behavior change... So for now, in the lockfile, we filter distributions that don't match the source index URL.

There are two cases to consider:

- There's a source distribution. Then, for the ID to reference the `--find-links` registry, the source distribution _must_ have come from the `--find-links` entry, so it's fine to discard any wheels from the "wrong" registry without breaking any compatibility guarantees.
- There's no source distribution. Then the best wheel must come from the `--find-links` registry. We might lose some platform coverage by discarding the other wheels, but it shouldn't break any of the "guarantees", since we have at least one wheel that fits in the version range.

Closes https://github.com/astral-sh/uv/issues/6015.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-08-12 02:16_

---

_Label `bug` added by @charliermarsh on 2024-08-12 02:16_

---

_Label `preview` added by @charliermarsh on 2024-08-12 02:16_

---

_Label `lock` added by @charliermarsh on 2024-08-12 02:16_

---

_@BurntSushi approved on 2024-08-12 12:52_

Makes sense to me!

---

_Merged by @charliermarsh on 2024-08-12 12:54_

---

_Closed by @charliermarsh on 2024-08-12 12:54_

---

_Branch deleted on 2024-08-12 12:54_

---
