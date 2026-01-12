```yaml
number: 11107
title: Ignore non-hash fragments in HTML API responses
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/allow-hash
created_at: 2025-01-30T18:17:07Z
updated_at: 2025-01-30T18:35:14Z
url: https://github.com/astral-sh/uv/pull/11107
synced_at: 2026-01-12T16:09:41Z
```

# Ignore non-hash fragments in HTML API responses

---

_@charliermarsh_

## Summary

I'm not a fan of registries including fragments here that aren't hashes, but the spec doesn't expressly forbid it. I think it's reasonable to ignore them.

Specifically, the spec is here: https://packaging.python.org/en/latest/specifications/simple-repository-api/. It says that:

> The URL **SHOULD** include a hash in the form of a URL fragment with the following syntax: `#<hashname>=<hashvalue>`, where `<hashname>`he lowercase name of the hash function (such as sha256) and `<hashvalue>` is the hex encoded digest.

But it doesn't mention other fragments.

Closes https://github.com/astral-sh/uv/issues/7257.


---

_Review requested from @BurntSushi by @charliermarsh on 2025-01-30 18:17_

---

_Review requested from @Gankra by @charliermarsh on 2025-01-30 18:17_

---

_Review requested from @zanieb by @charliermarsh on 2025-01-30 18:17_

---

_Review requested from @konstin by @charliermarsh on 2025-01-30 18:17_

---

_Label `compatibility` added by @charliermarsh on 2025-01-30 18:18_

---

_@BurntSushi approved on 2025-01-30 18:29_

---

_@zanieb approved on 2025-01-30 18:35_

---

_Merged by @zanieb on 2025-01-30 18:35_

---

_Closed by @zanieb on 2025-01-30 18:35_

---

_Branch deleted on 2025-01-30 18:35_

---
