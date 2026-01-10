```yaml
number: 2789
title: "Preserve `.git` suffixes and casing in Git dependencies"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/allowed-urls
created_at: 2024-04-03T00:12:43Z
updated_at: 2024-04-03T00:24:30Z
url: https://github.com/astral-sh/uv/pull/2789
synced_at: 2026-01-10T14:49:08Z
```

# Preserve `.git` suffixes and casing in Git dependencies

---

_Pull request opened by @charliermarsh on 2024-04-03 00:12_

## Summary

I noticed in #2769 that I was now stripping `.git` suffixes from Git URLs after resolving to a precise commit. This PR cleans up the internal caching to use a better canonical representation: a `RepositoryUrl` along with a `GitReference`, instead of a `GitUrl` which can contain non-canonical data. This gives us both better fidelity (preserving the `.git`, along with any casing that the user provided when defining the URL) and is overall cleaner and more robust.


---

_Label `bug` added by @charliermarsh on 2024-04-03 00:12_

---

_Marked ready for review by @charliermarsh on 2024-04-03 00:12_

---

_Renamed from "Preserve .git suffixes in Git dependencies" to "Preserve `.git` suffixes and casing in Git dependencies" by @charliermarsh on 2024-04-03 00:15_

---

_Merged by @charliermarsh on 2024-04-03 00:24_

---

_Closed by @charliermarsh on 2024-04-03 00:24_

---

_Branch deleted on 2024-04-03 00:24_

---
