```yaml
number: 3124
title: "refactor(ruff): Implement `doc_lines_from_tokens` as iterator"
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: refactor/doc-lines-iterator
created_at: 2023-02-22T14:06:43Z
updated_at: 2023-02-22T14:22:28Z
url: https://github.com/astral-sh/ruff/pull/3124
synced_at: 2026-01-12T15:55:12Z
```

# refactor(ruff): Implement `doc_lines_from_tokens` as iterator

---

_@MichaReiser_

This is a nit refactor... It implements the extraction of document lines as an iterator instead of a Vector to avoid the extra allocation.

---

_Review comment by @MichaReiser on `crates/ruff/src/fs.rs`:93 on 2023-02-22 14:10_

This fn is identical to `std::fs::read_to_string` 

---

_@MichaReiser reviewed on 2023-02-22 14:10_

---

_Merged by @charliermarsh on 2023-02-22 14:22_

---

_Closed by @charliermarsh on 2023-02-22 14:22_

---

_@charliermarsh reviewed on 2023-02-22 14:22_

---

_Review comment by @charliermarsh on `crates/ruff/src/fs.rs`:93 on 2023-02-22 14:22_

Lol, I hope it either _was_ different at one point, _or_ it was just one of the first few functions I wrote and I didn't realize they were the same.

---
