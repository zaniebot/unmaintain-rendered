---
number: 1974
title: "Does `--find-links <local-path> --offline` allow packages to be used?"
type: issue
state: closed
author: zanieb
labels:
  - question
assignees: []
created_at: 2024-02-25T22:20:44Z
updated_at: 2024-02-25T23:11:10Z
url: https://github.com/astral-sh/uv/issues/1974
synced_at: 2026-01-07T13:12:16-06:00
---

# Does `--find-links <local-path> --offline` allow packages to be used?

---

_Issue opened by @zanieb on 2024-02-25 22:20_

Does `uv` pull packages from e.g. a `--find-links .../site-packages` when `--offline` is used? I don't think so, I think it only reads from the cache — but it should allow loading from local paths right?

---

_Label `question` added by @zanieb on 2024-02-25 22:20_

---

_Comment by @charliermarsh on 2024-02-25 22:22_

I think it should allow that. Does it not?

---

_Comment by @zanieb on 2024-02-25 23:11_

I think it does

```
❯ uv pip install --no-index --find-links ~/workspace/packse/vendor/build --force-reinstall wheel --offline --no-cache
Resolved 1 package in 8ms
Downloaded 1 package in 5ms
Installed 1 package in 5ms
 - wheel==0.42.0
 + wheel==0.42.0
```

I think I got confused because I thought `--find-links` would find unpackaged distributions in `site-packages`.

---

_Closed by @zanieb on 2024-02-25 23:11_

---
