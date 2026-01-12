```yaml
number: 90
title: Check for updates
type: pull_request
state: merged
author: mgrachev
labels: []
assignees: []
merged: true
base: main
head: check-for-updates
created_at: 2022-09-03T09:46:39Z
updated_at: 2022-09-03T21:02:08Z
url: https://github.com/astral-sh/ruff/pull/90
synced_at: 2026-01-12T05:48:44Z
```

# Check for updates

---

_Pull request opened by @mgrachev on 2022-09-03 09:46_

Hi there!

I've decided to add this feature if you don't mind 🙂

Close: #81

---

_Comment by @charliermarsh on 2022-09-03 15:06_

Awesome, thanks! Does the informer cache these lookups? Or are we hitting PyPI every time? If not, can you think of a strategy to (e.g.) only check for updates once a day or similar?

---

_Comment by @mgrachev on 2022-09-03 18:32_

`update-informer` already does it all by default:
- [interval](https://github.com/mgrachev/update-informer#interval)
- [cache](https://github.com/mgrachev/update-informer#cache-file)

---

_Comment by @charliermarsh on 2022-09-03 20:31_

Cool, just tested thus out -- very nice! Thank you!

---

_Merged by @charliermarsh on 2022-09-03 20:31_

---

_Closed by @charliermarsh on 2022-09-03 20:31_

---

_Branch deleted on 2022-09-03 21:02_

---
