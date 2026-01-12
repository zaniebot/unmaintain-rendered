```yaml
number: 428
title: Improve PEP 691 compatibility
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: pep-691
created_at: 2023-11-15T18:53:09Z
updated_at: 2023-11-16T18:03:46Z
url: https://github.com/astral-sh/uv/pull/428
synced_at: 2026-01-12T16:03:56Z
```

# Improve PEP 691 compatibility

---

_@konstin_

[PEP 691](https://peps.python.org/pep-0691/#project-detail) has slightly different, more relaxed rules around file metadata. These changes are now reflected in the `File` struct. This will make it easier to support alternative indices.

I had expected that i need to introduce a separate type for that, so i'm happy it's two `Option`s more and an alias.

Part of #412

---

_Marked ready for review by @konstin on 2023-11-16 12:11_

---

_@konstin reviewed on 2023-11-16 12:12_

---

_Review comment by @konstin on `crates/pypi-types/src/simple_json.rs`:31 on 2023-11-16 12:12_

This one is handled in #434 to avoid merge conflicts

---

_@charliermarsh approved on 2023-11-16 17:45_

---

_Merged by @konstin on 2023-11-16 18:03_

---

_Closed by @konstin on 2023-11-16 18:03_

---

_Branch deleted on 2023-11-16 18:03_

---
