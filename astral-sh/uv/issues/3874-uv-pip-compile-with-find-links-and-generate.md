```yaml
number: 3874
title: uv pip compile with --find-links and --generate-hashes does not generate hashes for wheels (.whl)
type: issue
state: closed
author: mescanne
labels:
  - bug
  - enhancement
assignees: []
created_at: 2024-05-28T08:46:37Z
updated_at: 2024-07-29T08:49:40Z
url: https://github.com/astral-sh/uv/issues/3874
synced_at: 2026-01-10T04:53:49Z
```

# uv pip compile with --find-links and --generate-hashes does not generate hashes for wheels (.whl)

---

_Issue opened by @mescanne on 2024-05-28 08:46_

Working with uv and pip-compile I've noticed that pip-compile will generate a hash from the .whl file (found using -f), while uv pip compile will not include the hash from whl.

I'm happy to provide a more detailed example if it's useful.

Thanks!



---

_Label `enhancement` added by @charliermarsh on 2024-06-01 20:33_

---

_Comment by @charliermarsh on 2024-06-01 20:33_

Makes sense, thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-29 01:58_

---

_Label `bug` added by @charliermarsh on 2024-07-29 01:58_

---

_Closed by @charliermarsh on 2024-07-29 08:49_

---
