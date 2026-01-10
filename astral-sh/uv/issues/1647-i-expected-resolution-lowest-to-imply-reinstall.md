```yaml
number: 1647
title: "I expected `--resolution=lowest` to imply `--reinstall` for an existing package"
type: issue
state: closed
author: adamtheturtle
labels:
  - documentation
assignees: []
created_at: 2024-02-18T14:07:49Z
updated_at: 2024-04-23T19:45:37Z
url: https://github.com/astral-sh/uv/issues/1647
synced_at: 2026-01-10T05:31:36Z
```

# I expected `--resolution=lowest` to imply `--reinstall` for an existing package

---

_Issue opened by @adamtheturtle on 2024-02-18 14:07_

I ran:

```
uv pip install ruff # installs the latest version
> uv pip install --resolution=lowest ruff
Audited 1 package in 0ms
```

I expected that this would install the lowest version of ruff.
I think that either changing the behaviour, or documenting this is suitable.

---

_Comment by @charliermarsh on 2024-02-18 15:08_

I think our behavior here is actually okay, because it's consistent in general with how `pip install` works: if you have a satisfactory version in your environment, it defers to that. But we should document it somewhere / somehow.

---

_Label `documentation` added by @charliermarsh on 2024-02-18 15:08_

---

_Comment by @zanieb on 2024-02-18 17:05_

We could also consider displaying a warning? That might be hard to do well though.

---

_Comment by @charliermarsh on 2024-04-23 19:45_

I think our behavior here is ok, though I can understand why it might be confusing on first run.

---

_Closed by @charliermarsh on 2024-04-23 19:45_

---
