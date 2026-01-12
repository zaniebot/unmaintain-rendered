```yaml
number: 7371
title: Treat parenthesized power operands as non-simple
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/pow
created_at: 2023-09-13T23:00:00Z
updated_at: 2023-09-14T15:36:23Z
url: https://github.com/astral-sh/ruff/pull/7371
synced_at: 2026-01-12T02:39:09Z
```

# Treat parenthesized power operands as non-simple

---

_Pull request opened by @charliermarsh on 2023-09-13 23:00_

Closes https://github.com/astral-sh/ruff/issues/7318.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/binary_like.rs`:667 on 2023-09-13 23:00_

I kept this out of `is_simple_power_expression` to keep that method `const` and trivia-agnostic, but not sure that's correct.

---

_@charliermarsh reviewed on 2023-09-13 23:00_

---

_Label `formatter` added by @charliermarsh on 2023-09-13 23:00_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-13 23:06_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/binary_like.rs`:667 on 2023-09-14 06:25_

I would prefer it in `is_simple_power_expression`. The loop here is already complicated enough... Keeping it `const` isn't that important as it is a private function anyway.

---

_@MichaReiser approved on 2023-09-14 06:27_

Thanks for your first time contribution ;) 

---

_@konstin approved on 2023-09-14 07:43_

---

_Merged by @charliermarsh on 2023-09-14 15:36_

---

_Closed by @charliermarsh on 2023-09-14 15:36_

---

_Branch deleted on 2023-09-14 15:36_

---
