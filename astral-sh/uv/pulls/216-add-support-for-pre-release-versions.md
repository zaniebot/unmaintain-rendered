```yaml
number: 216
title: Add support for pre-release versions
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/pre-release
created_at: 2023-10-29T18:26:07Z
updated_at: 2023-10-30T13:37:55Z
url: https://github.com/astral-sh/uv/pull/216
synced_at: 2026-01-12T16:03:48Z
```

# Add support for pre-release versions

---

_@charliermarsh_

We now accept a pre-release if (1) all versions are pre-releases, or (2) there was a pre-release marker in the dependency specifiers for a direct dependency.

The code is written such that we can support a variety of pre-release strategies.

Closes https://github.com/astral-sh/puffin/issues/191.


---

_Label `enhancement` added by @charliermarsh on 2023-10-29 18:26_

---

_Merged by @charliermarsh on 2023-10-29 18:31_

---

_Closed by @charliermarsh on 2023-10-29 18:31_

---

_Branch deleted on 2023-10-29 18:31_

---

_Review comment by @konstin on `crates/puffin-resolver/src/candidate_selector.rs`:160 on 2023-10-30 11:27_

We should prefer a stable sdist over a prerelease wheel. It's slower but it's more correct

---

_@konstin reviewed on 2023-10-30 11:30_

---

_@charliermarsh reviewed on 2023-10-30 13:21_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/candidate_selector.rs`:160 on 2023-10-30 13:21_

I agree this needs changing, but does it not depend on which is a "better match"? If the pre-release wheel is a newer version than the stable sdist (but both match the specifier), and the user has Puffin configured to allow pre-releases for any package, shouldn't we pick the wheel?

---

_@konstin reviewed on 2023-10-30 13:35_

---

_Review comment by @konstin on `crates/puffin-resolver/src/candidate_selector.rs`:160 on 2023-10-30 13:35_

Yes, let me try to phrase it clearer: We should make the decision for selecting a version independent of whether there's a wheel or an sdist; From the perspective of pip or poetry, whether a version has a source or a built distribution is incidental, either means it can be installed and will be selected.

---

_@charliermarsh reviewed on 2023-10-30 13:37_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/candidate_selector.rs`:160 on 2023-10-30 13:37_

Yes, agree. Will refine a bit.

---
