```yaml
number: 1158
title: Add platform support to the README
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/platform
created_at: 2024-01-29T01:49:10Z
updated_at: 2024-01-29T13:37:13Z
url: https://github.com/astral-sh/uv/pull/1158
synced_at: 2026-01-12T16:04:28Z
```

# Add platform support to the README

---

_@charliermarsh_

Closes https://github.com/astral-sh/puffin/issues/1149.

---

_Review requested from @konstin by @charliermarsh on 2024-01-29 01:49_

---

_Label `documentation` added by @charliermarsh on 2024-01-29 01:49_

---

_@zanieb approved on 2024-01-29 03:31_

---

_Merged by @charliermarsh on 2024-01-29 03:52_

---

_Closed by @charliermarsh on 2024-01-29 03:52_

---

_Branch deleted on 2024-01-29 03:52_

---

_@konstin reviewed on 2024-01-29 07:39_

---

_Review comment by @konstin on `README.md`:278 on 2024-01-29 07:39_

I wouldn't list those separately, but if we do, we should document the glibc requirement(s) for non-musl linux.

---

_@charliermarsh reviewed on 2024-01-29 12:02_

---

_Review comment by @charliermarsh on `README.md`:278 on 2024-01-29 12:02_

Can you explain why? Are you referring specifically to the `x86_64` and `aarch64` architectures or all of these? (I followed Rust here which lists them separately.)

---

_@konstin reviewed on 2024-01-29 12:27_

---

_Review comment by @konstin on `README.md`:278 on 2024-01-29 12:27_

I wouldn't list musl separately, it's not really a separate os/arch and more a separate way of building/shipping puffin plus some extra musllinux tag handling. We could write something like "puffin requires at least glibc x.y for manylinux/glibc platforms. There is tier 2 support for musllinux."

---

_@charliermarsh reviewed on 2024-01-29 13:31_

---

_Review comment by @charliermarsh on `README.md`:278 on 2024-01-29 13:31_

Would it make sense to just say `Linux (armv7)`? So include the line item but omit musl entirely?

---

_@konstin reviewed on 2024-01-29 13:37_

---

_Review comment by @konstin on `README.md`:278 on 2024-01-29 13:37_

yes!

---
