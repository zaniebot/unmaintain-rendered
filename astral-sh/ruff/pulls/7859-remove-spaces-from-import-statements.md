```yaml
number: 7859
title: Remove spaces from import statements
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: import-spaces
created_at: 2023-10-09T08:18:20Z
updated_at: 2023-10-11T11:35:42Z
url: https://github.com/astral-sh/ruff/pull/7859
synced_at: 2026-01-12T02:32:41Z
```

# Remove spaces from import statements

---

_Pull request opened by @konstin on 2023-10-09 08:18_

**Summary** Remove spaces from import statements such as 

```python
import tqdm .  tqdm
from tqdm .    auto import tqdm
```

See also #7760 for a better solution.

**Test Plan** New fixtures

---

_Label `formatter` added by @konstin on 2023-10-09 08:18_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/identifier.rs`:59 on 2023-10-09 11:48_

Can we restore the existing comment, and add a second example to explain that there can be arbitrary whitespace between dots? The new comment feels like a less-useful subset of the existing comment.

---

_@charliermarsh reviewed on 2023-10-09 11:48_

---

_@charliermarsh reviewed on 2023-10-09 11:51_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/identifier.rs`:77 on 2023-10-09 11:51_

Annoying that we pass `&str` in here only for it to be converted to `String` internally when formatting, but I don't see any alternative APIs.

---

_@charliermarsh approved on 2023-10-09 11:51_

---

_@konstin reviewed on 2023-10-11 11:28_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/other/identifier.rs`:77 on 2023-10-11 11:28_

We could change the text element to use `Cow`, there are more places where we had owned strings previously.

---

_Merged by @konstin on 2023-10-11 11:35_

---

_Closed by @konstin on 2023-10-11 11:35_

---

_Branch deleted on 2023-10-11 11:35_

---
