```yaml
number: 574
title: "Remove `.whl` extension for cached, unzipped wheels"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ext
created_at: 2023-12-05T21:15:46Z
updated_at: 2023-12-05T22:41:23Z
url: https://github.com/astral-sh/uv/pull/574
synced_at: 2026-01-10T15:44:44Z
```

# Remove `.whl` extension for cached, unzipped wheels

---

_Pull request opened by @charliermarsh on 2023-12-05 21:15_

## Summary

This PR uses the wheel stem (e.g.,  `foo-1.2.3-py3-none-any`) instead of the wheel name (e.g., `foo-1.2.3-py3-none-any.whl`) when storing unzipped wheels in the cache, which removes a class of confusing issues around overwrites and directory-vs.-file collisions.

For now, we retain _both_ the zipped and unzipped wheels in the cache, though we can easily change this by storing the zipped wheels in a temporary directory.

Closes https://github.com/astral-sh/puffin/issues/573.

## Test Plan

Some examples from my local cache:

<img width="835" alt="Screen Shot 2023-12-05 at 4 09 55 PM" src="https://github.com/astral-sh/puffin/assets/1309177/784146aa-b080-416e-9767-40c843fe5d6a">
<img width="847" alt="Screen Shot 2023-12-05 at 4 12 14 PM" src="https://github.com/astral-sh/puffin/assets/1309177/4bc7f30f-bef3-47f1-b4e8-da9cabf87f28">
<img width="637" alt="Screen Shot 2023-12-05 at 4 09 50 PM" src="https://github.com/astral-sh/puffin/assets/1309177/25ca4944-4a06-4a08-ac85-c6f7d8b5c8ea">


---

_Review requested from @konstin by @charliermarsh on 2023-12-05 21:15_

---

_Renamed from "Remove .whl extension for cached, unzipped wheels" to "Remove `.whl` extension for cached, unzipped wheels" by @charliermarsh on 2023-12-05 21:15_

---

_Label `bug` added by @charliermarsh on 2023-12-05 21:15_

---

_@charliermarsh reviewed on 2023-12-05 21:16_

---

_Review comment by @charliermarsh on `crates/distribution-filename/src/wheel.rs`:41 on 2023-12-05 21:16_

@konstin - Do you know why we do this...? This is unchanged, Git is just confused, see it in red below.

---

_@charliermarsh reviewed on 2023-12-05 21:21_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/source_dist.rs`:98 on 2023-12-05 21:21_

We have to be careful with these structs because the thing at `target` may or may not exist (it's the unzipped wheel). But we're now planning to have semantics such that if it _does_ exist, it's respected (rather than deleted).

---

_@konstin reviewed on 2023-12-05 22:07_

---

_Review comment by @konstin on `crates/distribution-filename/src/wheel.rs`:41 on 2023-12-05 22:07_

iirc because `_` vs `-`, but that might not be applicable anymore

---

_@konstin approved on 2023-12-05 22:09_

Thanks!

The cache bucket docs need updating

---

_Comment by @charliermarsh on 2023-12-05 22:34_

Done, good call.

---

_Merged by @charliermarsh on 2023-12-05 22:41_

---

_Closed by @charliermarsh on 2023-12-05 22:41_

---

_Branch deleted on 2023-12-05 22:41_

---
