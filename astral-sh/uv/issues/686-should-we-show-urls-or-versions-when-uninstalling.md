```yaml
number: 686
title: Should we show URLs or versions when uninstalling URL-based dependencies?
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2023-12-18T15:02:34Z
updated_at: 2023-12-18T22:21:39Z
url: https://github.com/astral-sh/uv/issues/686
synced_at: 2026-01-12T15:58:24Z
```

# Should we show URLs or versions when uninstalling URL-based dependencies?

---

_@charliermarsh_

Right now, when we uninstall a distribution that was installed via a URL, we show the version rather than the direct URL. Is this wrong?

For example, if you _reinstall_ Black as editable, we show the removed Black as a version:

```
 - black==23.10.2.dev15+gf7cbe4a
 + black @ ../black
```

---

_Comment by @charliermarsh on 2023-12-18 15:48_

I tend to think we should show the URL.

---

_Comment by @zanieb on 2023-12-18 15:52_

Huh it's cool that we have the full version like that. We should probably use the URL... although I wouldn't mind both?

```
black==23.10.2.dev15+gf7cbe4a [from git+https://github.com/psf/black]
```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-18 16:55_

---

_Closed by @charliermarsh on 2023-12-18 22:21_

---
