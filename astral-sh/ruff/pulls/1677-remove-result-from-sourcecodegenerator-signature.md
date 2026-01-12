```yaml
number: 1677
title: Remove Result from SourceCodeGenerator signature
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/generator
created_at: 2023-01-06T02:11:16Z
updated_at: 2023-01-06T02:41:27Z
url: https://github.com/astral-sh/ruff/pull/1677
synced_at: 2026-01-12T05:36:32Z
```

# Remove Result from SourceCodeGenerator signature

---

_Pull request opened by @charliermarsh on 2023-01-06 02:11_

We populate this buffer ourselves, so I believe it's fine for us to use an unchecked UTF-8 cast here. It _dramatically_ simplifies so much downstream code.

---

_@andersk reviewed on 2023-01-06 02:12_

---

_Review comment by @andersk on `src/source_code_generator.rs`:62 on 2023-01-06 02:12_

This would be the only `unsafe` block in Ruff, which is unfortunate. Consider `.unwrap()` instead?

---

_@charliermarsh reviewed on 2023-01-06 02:18_

---

_Review comment by @charliermarsh on `src/source_code_generator.rs`:62 on 2023-01-06 02:18_

Yeah I'm totally fine with that.

---

_@charliermarsh reviewed on 2023-01-06 02:20_

---

_Review comment by @charliermarsh on `src/source_code_generator.rs`:62 on 2023-01-06 02:20_

Do you think it's a bad idea to fail hard here vs. propagating the error?

---

_@andersk reviewed on 2023-01-06 02:40_

---

_Review comment by @andersk on `src/source_code_generator.rs`:62 on 2023-01-06 02:40_

Panicking with `unwrap` or `expect` seems like a fine reaction to a bug that broke our internal invariants; the backtrace would make it clear what happened in that event.

---

_Merged by @charliermarsh on 2023-01-06 02:41_

---

_Closed by @charliermarsh on 2023-01-06 02:41_

---

_Branch deleted on 2023-01-06 02:41_

---
