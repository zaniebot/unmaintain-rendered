```yaml
number: 3159
title: "Add support for `--upgrade-strategy` or specify behavior"
type: issue
state: closed
author: luiscastro193
labels:
  - question
assignees: []
created_at: 2024-04-20T09:21:13Z
updated_at: 2024-04-21T08:28:34Z
url: https://github.com/astral-sh/uv/issues/3159
synced_at: 2026-01-12T15:58:42Z
```

# Add support for `--upgrade-strategy` or specify behavior

---

_@luiscastro193_

By default, `pip --upgrade` only upgrades packages specified on pip’s command line. Ideally, at least dependencies would also be considered. This can be fixed with `--upgrade-strategy eager`.

What is the default behavior of `uv`? If dependencies are not considered, could `--upgrade-strategy eager` be added as an option?

---

_Comment by @charliermarsh on 2024-04-20 13:31_

By default, we don't try to upgrade installed packages. But if you run with `--upgrade`, we attempt to upgrade all packages. You can also pass `--upgrade-package flask` to only upgrade a single package.

---

_Closed by @charliermarsh on 2024-04-20 13:31_

---

_Comment by @charliermarsh on 2024-04-20 13:31_

(Hopefully that helps but happy to answer any follow-ups.)

---

_Label `question` added by @charliermarsh on 2024-04-20 13:31_

---

_Comment by @luiscastro193 on 2024-04-20 16:19_

What do you mean by "we attempt to upgrade all packages"?

If you run `pip install --upgrade flask` it will only upgrade flask. However, if you run `pip install --upgrade --upgrade-strategy eager flask` it will upgrade flask and its requirements.

So what does `uv pip install --upgrade flask` do? Does it upgrade dependencies as well then?

---

_Comment by @luiscastro193 on 2024-04-21 08:28_

I did a quick test and `uv pip install --upgrade` upgrades indeed dependencies as well. That's great. Maybe it should be noted though in [PIP_COMPATIBILITY.md](../blob/main/PIP_COMPATIBILITY.md)?

---
