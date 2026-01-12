```yaml
number: 2489
title: Fix priority of ABI tags
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/abi-tags-fix
created_at: 2024-03-16T16:39:19Z
updated_at: 2024-03-18T14:21:55Z
url: https://github.com/astral-sh/uv/pull/2489
synced_at: 2026-01-12T16:05:04Z
```

# Fix priority of ABI tags

---

_@zanieb_

Brings us in-line with Python's behavior:

1. Prioritize `none` tags _after_ all of the relevant platform tags 
2. Omit  `none` tags for CPython versions less than the current version
3. Prioritize major (i.e. `py3-none`) version tags over minor (i.e. `py3x-none`) version tags less than the current version
4. Add a `none-any` tag for the current CPython version


## Test plan

Tested on my Linux machine with a script to emit tags at the desired glibc version:

```python
from packaging import tags
import re

exclude = re.compile("_(21|22|23|24|25|26|27|28|29|30|31|32|33|34|35|36|37|38|39)_")

for tag in tags.sys_tags():
    if exclude.search(str(tag)):
        continue
    print(tag)
```

Then performed a diff with the snapshot in `tags.rs`

---

_Label `bug` added by @zanieb on 2024-03-16 17:35_

---

_Comment by @zanieb on 2024-03-16 17:49_

I'd like to do some additional verification that changes are correct on other platforms too e.g. macOS

---

_Review requested from @charliermarsh by @zanieb on 2024-03-16 17:49_

---

_Review requested from @konstin by @zanieb on 2024-03-16 17:49_

---

_@charliermarsh approved on 2024-03-16 18:11_

---

_Comment by @zanieb on 2024-03-17 16:09_

I've confirmed this is correct on macOS

---

_@konstin approved on 2024-03-18 11:54_

---

_Merged by @zanieb on 2024-03-18 14:21_

---

_Closed by @zanieb on 2024-03-18 14:21_

---

_Branch deleted on 2024-03-18 14:21_

---
