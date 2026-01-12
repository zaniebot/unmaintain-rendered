```yaml
number: 5353
title: "Remove Simple API cache files for alternative indexes in `cache clean`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/cache-clean
created_at: 2024-07-23T17:31:31Z
updated_at: 2024-07-23T17:42:57Z
url: https://github.com/astral-sh/uv/pull/5353
synced_at: 2026-01-12T16:06:46Z
```

# Remove Simple API cache files for alternative indexes in `cache clean`

---

_@charliermarsh_

## Summary

The `simple-v9` directory was missing the `index` segment. See before:


![Screenshot 2024-07-23 at 1 29 18 PM](https://github.com/user-attachments/assets/3af9ccbf-ba45-4910-ad3b-4f52806dc8c9)

And after:

![Screenshot 2024-07-23 at 1 29 38 PM](https://github.com/user-attachments/assets/15535534-3304-4209-93e8-7f5e572929f0)

Every other bucket has this `index` segment for non-PyPI indexes, e.g.:

![Screenshot 2024-07-23 at 1 29 24 PM](https://github.com/user-attachments/assets/7354c9ad-7757-4a5f-a7b8-ff987a100e39)

Closes https://github.com/astral-sh/uv/issues/5352.


---

_Label `bug` added by @charliermarsh on 2024-07-23 17:31_

---

_Marked ready for review by @charliermarsh on 2024-07-23 17:36_

---

_Merged by @charliermarsh on 2024-07-23 17:42_

---

_Closed by @charliermarsh on 2024-07-23 17:42_

---

_Branch deleted on 2024-07-23 17:42_

---

_Comment by @charliermarsh on 2024-07-23 17:42_

Adding tests in a follow-up PR because I want to test more behavior.

---
