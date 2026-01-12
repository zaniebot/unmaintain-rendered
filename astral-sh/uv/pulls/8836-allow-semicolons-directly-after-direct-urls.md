```yaml
number: 8836
title: Allow semicolons directly after direct URLs
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/semi
created_at: 2024-11-05T18:36:09Z
updated_at: 2024-11-05T21:07:09Z
url: https://github.com/astral-sh/uv/pull/8836
synced_at: 2026-01-12T16:08:31Z
```

# Allow semicolons directly after direct URLs

---

_@charliermarsh_

## Summary

Like pip, we now allow the semicolon to directly proceed the URL (but require that it's either preceded or followed by a space):

```
# OK
./test.whl; sys_platform == 'darwin'

# OK
./test.whl ;sys_platform == 'darwin'

# Error
./test.whl;sys_platform == 'darwin'
```

Closes https://github.com/astral-sh/uv/issues/8831.


---

_Label `compatibility` added by @charliermarsh on 2024-11-05 18:36_

---

_Marked ready for review by @charliermarsh on 2024-11-05 18:36_

---

_@zanieb approved on 2024-11-05 19:42_

---

_Merged by @charliermarsh on 2024-11-05 21:07_

---

_Closed by @charliermarsh on 2024-11-05 21:07_

---

_Branch deleted on 2024-11-05 21:07_

---
