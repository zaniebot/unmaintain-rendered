```yaml
number: 2166
title: "Expose the `--exclude-newer` flag"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/exclude-newer
created_at: 2024-03-04T17:28:19Z
updated_at: 2024-03-04T17:51:20Z
url: https://github.com/astral-sh/uv/pull/2166
synced_at: 2026-01-12T16:04:53Z
```

# Expose the `--exclude-newer` flag

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/2088

---

_Label `enhancement` added by @zanieb on 2024-03-04 17:28_

---

_@charliermarsh reviewed on 2024-03-04 17:30_

---

_Review comment by @charliermarsh on `README.md`:360 on 2024-03-04 17:30_

Can you give an example here of a valid argument?

---

_@charliermarsh approved on 2024-03-04 17:30_

---

_Comment by @charliermarsh on 2024-03-04 17:31_

We may want to quickly check what happens for indexes etc. that don't provide a timestamp. I can't remember if we allow or disallow those distributions.

---

_Comment by @zanieb on 2024-03-04 17:32_

@charliermarsh I talk about that in the `README`? We ignore those distributions (which motivated https://github.com/devpi/devpi/pull/1023)

---

_Comment by @charliermarsh on 2024-03-04 17:38_

Thanks, I missed that.

---

_Merged by @zanieb on 2024-03-04 17:51_

---

_Closed by @zanieb on 2024-03-04 17:51_

---

_Branch deleted on 2024-03-04 17:51_

---
