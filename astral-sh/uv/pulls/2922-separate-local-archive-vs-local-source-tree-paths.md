```yaml
number: 2922
title: Separate local archive vs. local source tree paths in source database
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
  - internal
assignees: []
merged: true
base: main
head: charlie/source-tree
created_at: 2024-04-09T00:44:21Z
updated_at: 2024-04-09T01:12:34Z
url: https://github.com/astral-sh/uv/pull/2922
synced_at: 2026-01-12T16:05:18Z
```

# Separate local archive vs. local source tree paths in source database

---

_@charliermarsh_

## Summary

When you specify a source distribution via a path, it can either be a path to an archive (like a `.tar.gz` file), or a source tree (a directory). Right now, we handle both paths through the same methods in the source database. This PR splits them up into separate handlers.

This will make hash generation a little easier, since we need to generate hashes for archives, but _can't_ generate hashes for source trees.

It also means that we can now store the unzipped source distribution in the cache (in the case of archives), and avoid unzipping the source distribution needlessly on every invocation; and, overall, let's un enforce clearer expectations between the two routes (e.g., what errors are possible vs. not), at the cost of duplicating some code.

Closes #2760 (incidentally -- not exactly the motivation for the change, but it did accomplish it).


---

_Label `performance` added by @charliermarsh on 2024-04-09 00:44_

---

_Label `internal` added by @charliermarsh on 2024-04-09 00:44_

---

_Marked ready for review by @charliermarsh on 2024-04-09 01:04_

---

_Merged by @charliermarsh on 2024-04-09 01:12_

---

_Closed by @charliermarsh on 2024-04-09 01:12_

---

_Branch deleted on 2024-04-09 01:12_

---
