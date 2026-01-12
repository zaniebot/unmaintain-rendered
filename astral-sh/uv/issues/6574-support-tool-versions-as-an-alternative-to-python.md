```yaml
number: 6574
title: "Support `.tool-versions` as an alternative to `.python-versions`"
type: issue
state: open
author: TommasoAmici
labels:
  - enhancement
  - configuration
assignees: []
created_at: 2024-08-24T08:25:40Z
updated_at: 2024-08-25T02:36:36Z
url: https://github.com/astral-sh/uv/issues/6574
synced_at: 2026-01-12T15:59:05Z
```

# Support `.tool-versions` as an alternative to `.python-versions`

---

_@TommasoAmici_

For projects with multiple languages/tools, it's possible to have the python version in a `.tool-versions` file which is very similar in spirit to `.python-versions`, but allows defining all versions in a centralized configuration file. It'd be nice if `uv` supported that.

e.g.:
https://asdf-vm.com/manage/configuration.html#tool-versions
https://mise.jdx.dev/configuration.html#tool-versions

---

_Label `enhancement` added by @charliermarsh on 2024-08-25 02:36_

---

_Label `configuration` added by @charliermarsh on 2024-08-25 02:36_

---

_Comment by @charliermarsh on 2024-08-25 02:36_

This generally makes sense to me (though the syntax is more flexible, e.g., supports `sub` and stuff, so it's not totally trivial).

---
