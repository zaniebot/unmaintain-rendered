---
number: 20735
title: "[feature request] external dependencies in `ruff analyze graph`?"
type: issue
state: closed
author: gwdekker
labels:
  - question
  - analyze
assignees: []
created_at: 2025-10-07T06:38:04Z
updated_at: 2025-10-07T11:35:21Z
url: https://github.com/astral-sh/ruff/issues/20735
synced_at: 2026-01-10T01:23:01Z
---

# [feature request] external dependencies in `ruff analyze graph`?

---

_Issue opened by @gwdekker on 2025-10-07 06:38_

`ruff analyze graph` is amazing! What do you guys think about adding external dependencies as well? I found some prior art here: https://grimp.readthedocs.io/en/stable/usage.html, and I am pretty sure I saw the maintainer of that package (@seddonym) here on the issue tracker as well - so there might be some good collaboration opportunity here. Thanks!

---

_Comment by @MichaReiser on 2025-10-07 09:43_

This should already be supported. But you have to tell ruff where to find the venv by using `--python`

---

_Label `question` added by @MichaReiser on 2025-10-07 09:43_

---

_Label `analyze` added by @MichaReiser on 2025-10-07 09:43_

---

_Comment by @gwdekker on 2025-10-07 10:46_

A thanks! So `ruff analyze graph --python .venv/bin/python` seems indeed to do the trick.

I would argue that it is not very obvious from a user perspective that this is a possibility - and to me it feels a bit odd for an astral tool to need to get so explicit about the location of a virtualenv, normally you guys are doing a great job letting people forget about the existence of virtual envs to start with. So maybe this could be improved from that perspective. But it is awesome that the underlying functionality is there and available, thanks!

---

_Comment by @MichaReiser on 2025-10-07 11:35_

> I would argue that it is not very obvious from a user perspective that this is a possibility - and to me it feels a bit odd for an astral tool to need to get so explicit about the location of a virtualenv, normally you guys are doing a great job letting people forget about the existence of virtual envs to start with

We cold to port some of the detection logic over from ty.

---

_Closed by @MichaReiser on 2025-10-07 11:35_

---
