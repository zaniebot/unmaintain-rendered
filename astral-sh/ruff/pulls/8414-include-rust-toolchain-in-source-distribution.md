```yaml
number: 8414
title: Include rust-toolchain in source distribution
type: pull_request
state: merged
author: konstin
labels:
  - release
assignees: []
merged: true
base: main
head: include-rust-toolchain
created_at: 2023-11-01T17:38:51Z
updated_at: 2023-11-01T19:12:37Z
url: https://github.com/astral-sh/ruff/pull/8414
synced_at: 2026-01-10T23:40:55Z
```

# Include rust-toolchain in source distribution

---

_Pull request opened by @konstin on 2023-11-01 17:38_

**Summary** Simplify CI by ensuring that the source distribution is always built with the rust version that has been explicitly tested. See discussion in #8389

Closes #8389

---

_Review requested from @charliermarsh by @konstin on 2023-11-01 17:38_

---

_Review requested from @zanieb by @konstin on 2023-11-01 17:38_

---

_Assigned to @zanieb by @konstin on 2023-11-01 17:38_

---

_Comment by @stinodego on 2023-11-01 17:53_

Maybe you can also remove this line in the CI, as it's no longer needed:
https://github.com/astral-sh/ruff/blob/3fc920cd1277b31125c7f03bd50ee73259b58386/.github/workflows/release.yaml#L51

Then I can close [my PR](https://github.com/astral-sh/ruff/pull/8389#issuecomment-1789349037).

---

_Comment by @zanieb on 2023-11-01 18:22_

@stinodego done; added you as a co-author. Thanks!

---

_Label `release` added by @zanieb on 2023-11-01 18:22_

---

_@zanieb approved on 2023-11-01 18:51_

---

_Merged by @zanieb on 2023-11-01 18:51_

---

_Closed by @zanieb on 2023-11-01 18:51_

---

_Branch deleted on 2023-11-01 18:51_

---

_Comment by @stinodego on 2023-11-01 19:12_

> @stinodego done; added you as a co-author. Thanks!

Thank you both, I learned something new and can now improve the Polars sdist and CI!

---
