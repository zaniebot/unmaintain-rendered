```yaml
number: 4285
title: Use separate path types for directories and files
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/dir
created_at: 2024-06-12T18:07:28Z
updated_at: 2024-06-12T19:59:21Z
url: https://github.com/astral-sh/uv/pull/4285
synced_at: 2026-01-10T13:54:02Z
```

# Use separate path types for directories and files

---

_Pull request opened by @charliermarsh on 2024-06-12 18:07_

## Summary

This is what I consider to be the "real" fix for #8072. We now treat directory and path URLs as separate `ParsedUrl` types and `RequirementSource` types. This removes a lot of `.is_dir()` forking within the `ParsedUrl::Path` arms and makes some states impossible (e.g., you can't have a `.whl` path that is editable). It _also_ fixes the `direct_url.json` for direct URLs that refer to files. Previously, we wrote out to these as if they were installed as directories, which is just wrong.


---

_Label `internal` added by @charliermarsh on 2024-06-12 18:07_

---

_@charliermarsh reviewed on 2024-06-12 18:08_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/satisfies.rs`:254 on 2024-06-12 18:08_

E.g., this only applies to directories, not files.

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/unnamed.rs`:142 on 2024-06-12 18:08_

This condition is now encoded in the types directly.

---

_@charliermarsh reviewed on 2024-06-12 18:08_

---

_@charliermarsh reviewed on 2024-06-12 18:09_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/lowering.rs`:247 on 2024-06-12 18:09_

Not really a huge fan of _where_ we're detecting this but I don't know that we have much of a choice.

---

_@zanieb approved on 2024-06-12 18:11_

Big one

---

_@zanieb reviewed on 2024-06-12 18:12_

---

_Review comment by @zanieb on `crates/uv-distribution/src/metadata/lowering.rs`:247 on 2024-06-12 18:12_

Is there any other safe guard / enforcement?

---

_Merged by @charliermarsh on 2024-06-12 19:59_

---

_Closed by @charliermarsh on 2024-06-12 19:59_

---

_Branch deleted on 2024-06-12 19:59_

---
