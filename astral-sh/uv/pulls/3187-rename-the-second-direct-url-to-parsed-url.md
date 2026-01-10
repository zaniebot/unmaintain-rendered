```yaml
number: 3187
title: Rename the second direct url to parsed url
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/rename-second-direct-url-parsed-url
created_at: 2024-04-22T14:26:17Z
updated_at: 2024-04-22T14:38:28Z
url: https://github.com/astral-sh/uv/pull/3187
synced_at: 2026-01-10T14:43:32Z
```

# Rename the second direct url to parsed url

---

_Pull request opened by @konstin on 2024-04-22 14:26_

Previously, we got `pypi_types::DirectUrl` (the pypa spec direct_url.json format) and `distribution_types::DirectUrl` (an enum of all the url types we support). This lead me to confusion, so i'm renaming the latter one to the more appropriate `ParsedUrl`.

---

_Label `internal` added by @konstin on 2024-04-22 14:26_

---

_@charliermarsh reviewed on 2024-04-22 14:28_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/direct_url.rs`:22 on 2024-04-22 14:28_

Lets use "We" instead of "uv".

---

_@charliermarsh approved on 2024-04-22 14:28_

Reasonable rename.

---

_Merged by @konstin on 2024-04-22 14:38_

---

_Closed by @konstin on 2024-04-22 14:38_

---

_Branch deleted on 2024-04-22 14:38_

---
