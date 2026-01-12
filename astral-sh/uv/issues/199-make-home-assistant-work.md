```yaml
number: 199
title: Make home assistant work
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-10-26T13:22:19Z
updated_at: 2023-11-16T12:54:29Z
url: https://github.com/astral-sh/uv/issues/199
synced_at: 2026-01-12T15:58:22Z
```

# Make home assistant work

---

_@konstin_

Re https://github.com/astral-sh/puffin/issues/181#issuecomment-1780426042, when i merge their requirements we fail because of lack of support of in tree build backends. The other thing we don't support are constraints files, but ihmo it's fine to merge that in.


---

_Added to milestone `Feature complete` by @charliermarsh on 2023-10-26 13:30_

---

_Label `bug` added by @charliermarsh on 2023-10-26 13:30_

---

_Assigned to @konstin by @charliermarsh on 2023-10-26 13:30_

---

_Comment by @charliermarsh on 2023-11-15 23:21_

@konstin - I can't remember, does this work now?

---

_Comment by @konstin on 2023-11-16 12:54_

`puffin pip-compile requirements_test.txt` works, but otherwise their setup is too custom to be a good testing ground

---

_Closed by @konstin on 2023-11-16 12:54_

---
