```yaml
number: 1380
title: Only re-associate inline comments during normalization when necessary
type: pull_request
state: merged
author: squiddy
labels: []
assignees: []
merged: true
base: main
head: isort-inline-comment-handling
created_at: 2022-12-26T07:59:03Z
updated_at: 2022-12-27T05:42:30Z
url: https://github.com/astral-sh/ruff/pull/1380
synced_at: 2026-01-12T15:55:06Z
```

# Only re-associate inline comments during normalization when necessary

---

_@squiddy_

This is in preparation for the force-single-line isort setting. We need
to keep inline comments associated to the import itself, if possible.

This is the case when an `import from` statement only imports a single
name, there is no ambiguity as to where that comment belongs.

Input:

```python
from D import a_long_name_to_force_multiple_lines # Comment 12
from D import another_long_name_to_force_multiple_lines # Comment 13
```

Before:
```python
from D import (  # Comment 12  # Comment 13
    a_long_name_to_force_multiple_lines,
    another_long_name_to_force_multiple_lines,
)
```

After:
```python
from D import (
    a_long_name_to_force_multiple_lines,  # Comment 12
    another_long_name_to_force_multiple_lines,  # Comment 13
)
```

---

Note to reviewers:

I'm currently cloning the comment values not because I want to, but because I couldn't find a way to convince the compiler that it's fine (at least right now). Would appreciate any help here on how to express that better.

---

_Comment by @charliermarsh on 2022-12-26 12:51_

I was able to remove the clone, though not sure if the code became more or less clear...

---

_Merged by @charliermarsh on 2022-12-26 12:52_

---

_Closed by @charliermarsh on 2022-12-26 12:52_

---

_Branch deleted on 2022-12-27 05:42_

---
