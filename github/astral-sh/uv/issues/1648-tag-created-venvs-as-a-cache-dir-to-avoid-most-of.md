---
number: 1648
title: Tag created venvs as a cache dir to avoid most of in-project venv downsides
type: issue
state: closed
author: hynek
labels:
  - enhancement
assignees: []
created_at: 2024-02-18T14:31:50Z
updated_at: 2024-02-18T18:32:12Z
url: https://github.com/astral-sh/uv/issues/1648
synced_at: 2026-01-07T13:12:16-06:00
---

# Tag created venvs as a cache dir to avoid most of in-project venv downsides

---

_Issue opened by @hynek on 2024-02-18 14:31_

Some people prefer their virtualenvs in a central place which has its valid reasons.

Some of those reasons can be avoided by tagging the .venv directory as a cache directory which will be treated as such by software that supports it (e.g. backups).

Here's the standard: https://bford.info/cachedir/ but it's just adding a file called `CACHEDIR.TAG` with the contents `Signature: 8a477f597d28d172789f06886806bc55`.

The standard is surprisingly old (2004!), but I've noticed that Direnv has started adding it to its directories only recent and Brett Cannon asked about it re venv the other day too. So it looks like it might have some legs, finally.

---

_Comment by @charliermarsh on 2024-02-18 14:53_

Oh nice. We already add `CACHEDIR.TAG` to the uv cache. It seems correct to me to add this to virtualenvs too...

Any objections?

---

_Label `enhancement` added by @charliermarsh on 2024-02-18 14:53_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-18 15:27_

---

_Referenced in [astral-sh/uv#1653](../../astral-sh/uv/pulls/1653.md) on 2024-02-18 15:31_

---

_Closed by @charliermarsh on 2024-02-18 18:32_

---
