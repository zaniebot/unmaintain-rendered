```yaml
number: 3374
title: Add branch and tag variants to Git reference
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - preview
assignees: []
merged: true
base: main
head: charlie/g
created_at: 2024-05-04T18:36:18Z
updated_at: 2024-05-06T11:36:00Z
url: https://github.com/astral-sh/uv/pull/3374
synced_at: 2026-01-10T14:37:54Z
```

# Add branch and tag variants to Git reference

---

_Pull request opened by @charliermarsh on 2024-05-04 18:36_

## Summary

Closes https://github.com/astral-sh/uv/issues/3368.


---

_Label `internal` added by @charliermarsh on 2024-05-04 18:36_

---

_Label `preview` added by @charliermarsh on 2024-05-04 18:36_

---

_Review requested from @konstin by @charliermarsh on 2024-05-04 18:36_

---

_Marked ready for review by @charliermarsh on 2024-05-04 18:36_

---

_@charliermarsh reviewed on 2024-05-04 20:53_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:8646 on 2024-05-04 20:53_

This should say "failed to fetch tag". I guess this is because we lose the Git source when we convert back to a URL somewhere? I think we still need to do a refactor here to preserve these Git sources throughout, and avoid ever casting back to `Url` @konstin.

---

_Merged by @charliermarsh on 2024-05-04 21:13_

---

_Closed by @charliermarsh on 2024-05-04 21:13_

---

_Branch deleted on 2024-05-04 21:13_

---

_@konstin reviewed on 2024-05-06 11:36_

---

_Review comment by @konstin on `crates/uv/tests/pip_compile.rs`:8646 on 2024-05-06 11:36_

I think it's the case again where `GitSourceDist` only has a verbatim url, so it losses the more detailed information we got/parsed before in the bottleneck of `to_pubgrub` -> `PubGrubPackage::Package` -> `Dist::from_url`

---
