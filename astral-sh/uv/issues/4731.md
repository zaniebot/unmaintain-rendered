```yaml
number: 4731
title: "`uv tool` should support man pages"
type: issue
state: open
author: zanieb
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2024-07-02T15:23:06Z
updated_at: 2024-09-05T07:58:44Z
url: https://github.com/astral-sh/uv/issues/4731
synced_at: 2026-01-10T04:45:09Z
```

# `uv tool` should support man pages

---

_Issue opened by @zanieb on 2024-07-02 15:23_

pipx supports installation of man pages for documentation. We should do the same in the `uv tool` commands.

---

_Label `enhancement` added by @zanieb on 2024-07-02 15:23_

---

_Label `help wanted` added by @zanieb on 2024-07-02 15:23_

---

_Label `preview` added by @zanieb on 2024-07-02 15:23_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:22_

---

_Comment by @harkabeeparolus on 2024-09-02 05:13_

Agreed. This is the biggest feature that's still keeping me on pipx right now.

---

_Comment by @eth3lbert on 2024-09-05 07:58_

> Agreed. This is the biggest feature that's still keeping me on pipx right now.

Before this feature is fully implemented, you can use the following workaround: `uv tool run --from package man package`.

e.g., `uv tool run --from pycowsay man pycowsay`.




---
