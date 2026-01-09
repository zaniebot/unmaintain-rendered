---
number: 7569
title: "Allow updating `.python-version` and `.python-versions` files to latest patch version"
type: issue
state: open
author: zanieb
labels:
  - enhancement
  - cli
assignees: []
created_at: 2024-09-20T02:26:12Z
updated_at: 2024-09-23T13:41:10Z
url: https://github.com/astral-sh/uv/issues/7569
synced_at: 2026-01-07T13:12:17-06:00
---

# Allow updating `.python-version` and `.python-versions` files to latest patch version

---

_Issue opened by @zanieb on 2024-09-20 02:26_

e.g. we're out of date here in uv but it seems like a pain to figure out what the latest patch version is for each minor release. A command should do this for me :)

---

_Label `enhancement` added by @zanieb on 2024-09-20 02:26_

---

_Label `cli` added by @zanieb on 2024-09-20 02:26_

---

_Comment by @tfsingh on 2024-09-23 06:50_

Hey, I'm working on this [here](https://github.com/astral-sh/uv/compare/main...tfsingh:uv:tfsingh/patch) (still need to clean up code/docs, but I think the scaffolding is there), is there any particular source of truth/API you'd prefer I use to determine the latest patch version for each minor release? Thanks!

---

_Comment by @zanieb on 2024-09-23 13:41_

Cool! I think we'd use the latest patch version we can find in our downloads unless downloads are disabled, then we might need to use the latest version we can find on the system?

---

_Referenced in [astral-sh/uv#7678](../../astral-sh/uv/pulls/7678.md) on 2024-09-25 05:21_

---
