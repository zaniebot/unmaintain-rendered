```yaml
number: 749
title: Preserve comments when sorting imports
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/isort-comments
created_at: 2022-11-15T05:09:01Z
updated_at: 2022-11-16T03:02:53Z
url: https://github.com/astral-sh/ruff/pull/749
synced_at: 2026-01-12T15:55:05Z
```

# Preserve comments when sorting imports

---

_@charliermarsh_

It's tough to come up with a strategy that does the "right" thing in all cases (in part, because it's hard to even define the "right" output in every case), but this PR preserves _all_ comments, is stable across re-runs, and tries to do sensible things when merging across imports.

Resolves #675.


---

_Comment by @charliermarsh on 2022-11-15 05:12_

I'll add a few examples here tomorrow.

---

_Comment by @andersk on 2022-11-15 05:26_

There’s a slight Black incompatibility—Black requires this newline:

```python
import math
import os

# a comment
import pathlib
import sys
```

while Ruff deletes it:

```python
import math
import os
# a comment
import pathlib
import sys
```

---

_Comment by @andersk on 2022-11-15 05:40_

Comments seem to be unexpectedly counted toward the length of the following line in some cases, causing the following import to wrap early:

```python
import a
# ccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc
from b import c
```

→

```python
import a
# ccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc
from b import (
    c,
)
```

---

_Comment by @charliermarsh on 2022-11-15 05:46_

Woo thanks for trying! I noticed the second issue when running on Zulip :)

It's late here so will fix these both tomorrow (then hopefully merge).


---

_Comment by @andersk on 2022-11-15 05:47_

Multi-line comments are unexpectedly sorted and deduplicated:

```python
import io
# Old MacDonald had a farm,
# EIEIO
# And on his farm he had a cow,
# EIEIO
# With a moo-moo here and a moo-moo there
# Here a moo, there a moo, everywhere moo-moo
# Old MacDonald had a farm,
# EIEIO
from errno import EIO
```

→

```python
import io
# And on his farm he had a cow,
# EIEIO
# Here a moo, there a moo, everywhere moo-moo
# Old MacDonald had a farm,
# With a moo-moo here and a moo-moo there
from errno import (
    EIO,
)
```

---

_Comment by @charliermarsh on 2022-11-16 02:27_

Ok, I believe all of those are addressed. Here's the diff on Zulip:

![Screen Shot 2022-11-15 at 9 27 05 PM](https://user-images.githubusercontent.com/1309177/202068597-f6638e5e-7ec3-41bc-81d1-9f1218eca4b2.png)


---

_Merged by @charliermarsh on 2022-11-16 03:02_

---

_Closed by @charliermarsh on 2022-11-16 03:02_

---

_Branch deleted on 2022-11-16 03:02_

---
