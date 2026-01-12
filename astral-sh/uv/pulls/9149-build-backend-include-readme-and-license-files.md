```yaml
number: 9149
title: "Build backend: Include readme and license files"
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/build-backend-files
created_at: 2024-11-15T13:35:10Z
updated_at: 2024-11-15T14:41:40Z
url: https://github.com/astral-sh/uv/pull/9149
synced_at: 2026-01-12T16:08:40Z
```

# Build backend: Include readme and license files

---

_@konstin_

When building source distributions, we need to include the readme, so it can become part the METADATA body when building the wheel. We also need to support the license files from PEP 639. When building the source distribution, we copy those file relative to their origin, when building the wheel, we copy them to `.dist-info/licenses`.

The test for idempotence in wheel building is merged into the file listing test, which also covers that source tree -> source dist -> wheel is equivalent to source tree -> wheel, though we do need to check for file inclusion stronger here.

Best reviewed commit-by-commit

---

_Label `preview` added by @konstin on 2024-11-15 13:35_

---

_Review requested from @BurntSushi by @konstin on 2024-11-15 13:35_

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/metadata.rs`:591 on 2024-11-15 13:49_

Do you need access to everything? Would it be reasonable to expose only what you need through methods?

(I suggest this for abstract "encapsulation is good" reasons, and nothing in particular that is concrete. And I do sympathize with just making internals crate-public as that can often be easier.)

---

_Review comment by @BurntSushi on `scripts/packages/built-by-uv/third-party-licenses/PEP-401.txt`:4 on 2024-11-15 13:57_

Lol! https://peps.python.org/pep-0401/

---

_@BurntSushi approved on 2024-11-15 13:58_

Looks reasonable to me!

---

_Merged by @konstin on 2024-11-15 14:41_

---

_Closed by @konstin on 2024-11-15 14:41_

---

_Branch deleted on 2024-11-15 14:41_

---
