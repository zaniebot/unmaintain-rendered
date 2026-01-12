```yaml
number: 14056
title: Promote uv in installation guides
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - documentation
assignees: []
merged: true
base: main
head: main
created_at: 2024-11-02T18:03:19Z
updated_at: 2024-12-09T08:25:18Z
url: https://github.com/astral-sh/ruff/pull/14056
synced_at: 2026-01-12T15:55:46Z
```

# Promote uv in installation guides

---

_@InSyncWithFoo_

> [Because this is an Astral repository ;)](https://github.com/astral-sh/packse/pull/183)

[Originally reported](https://discord.com/channels/1039017663004942429/1039017663512449056/1302319421204729906) by clearfram3 on Discord.

`grep`ping for `pip install` in `.md` files reveals a few other places where the same fix might be applicable.

---

_Review requested from @zanieb by @charliermarsh on 2024-11-02 19:58_

---

_Label `documentation` added by @MichaReiser on 2024-11-03 11:52_

---

_@MichaReiser approved on 2024-12-08 18:57_

Thank you. I think this is fine. 

---

_@MichaReiser reviewed on 2024-12-08 18:58_

@zanieb what's our recommendation for installing tool? Should the example show `uvx` or use `uv add` to motivate adding it as a project dependency?

---

_Comment by @zanieb on 2024-12-08 22:02_

I'd say `uv add --dev ruff` to add it to your project and `uv tool install ruff` to install it globally.

---

_Review comment by @MichaReiser on `README.md`:124 on 2024-12-09 08:21_

```suggestion
uv add --dev ruff     # to add ruff to your project
uv tool install ruff  # to install ruff globally
```

---

_Review comment by @MichaReiser on `docs/faq.md`:228 on 2024-12-09 08:21_

```suggestion
# With uv.
$ uv add --dev ruff     # to add ruff to your project
$ uv tool install ruff  # to install ruff globally

# With pip
$ pip install ruff
```

---

_@MichaReiser approved on 2024-12-09 08:22_

---

_Merged by @MichaReiser on 2024-12-09 08:25_

---

_Closed by @MichaReiser on 2024-12-09 08:25_

---
