```yaml
number: 333
title: support pep593 annotations
type: pull_request
state: merged
author: adriangb
labels: []
assignees: []
merged: true
base: main
head: pep539-support
created_at: 2022-10-06T01:58:32Z
updated_at: 2022-10-06T16:05:13Z
url: https://github.com/astral-sh/ruff/pull/333
synced_at: 2026-01-12T15:55:04Z
```

# support pep593 annotations

---

_@adriangb_

Currently PEP593 annotations throw an error because they are being processed as regular type annotations where instead the first argument should be processed as a regular annotations and the rest should be processed as arbitrary code (and can have string constants, which is what causes the current error).

---

_@adriangb reviewed on 2022-10-06 02:02_

---

_Review comment by @adriangb on `src/check_ast.rs`:700 on 2022-10-06 02:02_

As a side note: it's totally valid to do `from typing import Literal as Foo`, I think this is just checking the name of the variable right?

---

_Comment by @charliermarsh on 2022-10-06 02:19_

Thanks for this! Excited to review.

---

_@charliermarsh reviewed on 2022-10-06 13:15_

---

_Review comment by @charliermarsh on `src/check_ast.rs`:700 on 2022-10-06 13:15_

Yeah that's right. These checks are a bit limited in that they don't follow reassignment for these identifiers.

---

_Merged by @charliermarsh on 2022-10-06 13:16_

---

_Closed by @charliermarsh on 2022-10-06 13:16_

---

_Comment by @charliermarsh on 2022-10-06 13:16_

Great PR! Thanks for putting it together! I made a couple minor tweaks but nothing major.

---

_Branch deleted on 2022-10-06 16:04_

---

_Review comment by @adriangb on `src/check_ast.rs`:134 on 2022-10-06 16:05_

This can definitely be refactored to avoid duplication, I got lazy 😴 

---

_@adriangb reviewed on 2022-10-06 16:05_

---
