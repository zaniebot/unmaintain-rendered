```yaml
number: 1378
title: "Add uv pip install git+ssh:// new feature"
type: issue
state: closed
author: gcamargo1
labels: []
assignees: []
created_at: 2024-02-15T23:05:50Z
updated_at: 2024-03-22T21:23:22Z
url: https://github.com/astral-sh/uv/issues/1378
synced_at: 2026-01-10T05:40:31Z
```

# Add uv pip install git+ssh:// new feature

---

_Issue opened by @gcamargo1 on 2024-02-15 23:05_

Hi,
Is it possible to add the :
`pip install git+ssh://`

capability?
Thank you.

---

_Comment by @charliermarsh on 2024-02-15 23:10_

👍 I believe this is the same as https://github.com/astral-sh/uv/issues/313. (We do support `flask @ git+https://github.com/pallets/flask.git@refs/pull/5313/head`, but we currently require the `flask @`.)

---

_Comment by @charliermarsh on 2024-02-15 23:10_

(I'm gonna merge into that issue.)

---

_Closed by @charliermarsh on 2024-02-15 23:10_

---

_Comment by @charliermarsh on 2024-03-22 21:23_

This is supported as of `v0.1.24`. You can `uv pip install git+ssh://...` directly, without including a package name.

---
