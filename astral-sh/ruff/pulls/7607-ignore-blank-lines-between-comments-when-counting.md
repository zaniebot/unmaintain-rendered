```yaml
number: 7607
title: Ignore blank lines between comments when counting newlines-after-imports
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: charlie/import
created_at: 2023-09-22T17:29:48Z
updated_at: 2023-09-22T17:50:08Z
url: https://github.com/astral-sh/ruff/pull/7607
synced_at: 2026-01-12T15:55:24Z
```

# Ignore blank lines between comments when counting newlines-after-imports

---

_@charliermarsh_

## Summary

Given:

```python
# -*- coding: utf-8 -*-
import random

# Defaults for arguments are defined here
# args.threshold = None;


logger = logging.getLogger("FastProject")
```

We want to count the number of newlines after `import random`, to ensure that there's _at least one_, but up to two.

Previously, we used the end range of the statement (then skipped trivia); instead, we need to use the end of the _last comment_. This is similar to #7556.

Closes https://github.com/astral-sh/ruff/issues/7604.


---

_Label `bug` added by @charliermarsh on 2023-09-22 17:29_

---

_Label `formatter` added by @charliermarsh on 2023-09-22 17:29_

---

_Comment by @codspeed-hq[bot] on 2023-09-22 17:40_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/import)

### Merging #7607 will **not alter performance**

<sub>Comparing <code>charlie/import</code> (0229d14) with <code>main</code> (8bfe9bd)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_@konstin approved on 2023-09-22 17:41_

---

_Merged by @charliermarsh on 2023-09-22 17:49_

---

_Closed by @charliermarsh on 2023-09-22 17:49_

---

_Branch deleted on 2023-09-22 17:49_

---
