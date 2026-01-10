---
number: 3747
title: "Where does `uv` land on PEP 517 `backend-path` out-of-tree?"
type: issue
state: closed
author: thejcannon
labels:
  - compatibility
assignees: []
created_at: 2024-05-22T17:44:26Z
updated_at: 2024-09-09T15:06:45Z
url: https://github.com/astral-sh/uv/issues/3747
synced_at: 2026-01-10T01:23:30Z
---

# Where does `uv` land on PEP 517 `backend-path` out-of-tree?

---

_Issue opened by @thejcannon on 2024-05-22 17:44_

👋 Ok, you caught me doing some weird things...

In a Python monorepo, with your own PEP 517/660 build backend, it's _really_ convenient to define your build-backend as:

```toml
[build-system]
requires = []
build-backend = "super_cool_custom_thing"
backend-path = [".."]
```

Unfortunately `pip` gets really angry when you do this.

But `uv` doesn't! 🎉 

So, I'm wondering if this is on purpose or on accident.

---

FWIW According to PEP 517:

> Directories in backend-path are interpreted as relative to the project root, and MUST refer to a location within the source tree (after relative paths and symbolic links have been resolved).

So I'm defintely violating it. But also:

> Frontends SHOULD check this condition

So you aren't _required_ by spec to error here. Hows about we slip `uv` a fiver to keep looking the other way? 😛 

(Or if you are planning on erroring, maybe allowing an opt-out flag? `UV_IM_VIOLATING_PEP_517_BACKEND_PATH_AND_I_HAVE_NO_SHAME=1`?)

---

_Comment by @thejcannon on 2024-05-22 17:46_

Reasons doing this is actually a good idea ™️ :
- You don't need to have your own cheeseshop for your build backend
- Alternatively, you don't bring in all the issues with `--no-build-isolation` on your 1p packages (e.g. you have to install your build backend first, and you can't install deps at the same time as your packages because you might not have installed your dependencies' build backends)
- You can feel safe knowing everyone is using your build backend (including the right version)

---

_Label `compatibility` added by @zanieb on 2024-05-22 17:51_

---

_Referenced in [astral-sh/uv#3796](../../astral-sh/uv/issues/3796.md) on 2024-05-23 14:52_

---

_Comment by @thejcannon on 2024-05-28 16:33_

(I have opened [a discussion related to this on Discourse](https://discuss.python.org/t/two-extensions-clarifications-of-pep-517/54464), but more generally)

---

_Comment by @charliermarsh on 2024-09-09 00:52_

I'm ok not to enforce this.

---

_Closed by @charliermarsh on 2024-09-09 00:52_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-09 00:52_

---

_Comment by @thejcannon on 2024-09-09 11:16_

Wonderful news!

If you're willing to write down what led you to this decision, for posterity, be my guest 😁

---

_Comment by @charliermarsh on 2024-09-09 13:44_

Mostly just that it costs us nothing and users are using it so it'd be breaking for us to remove :)

---

_Comment by @thejcannon on 2024-09-09 15:06_

I'll take it! If you give me a pointer, I'll file myself a task to add a test here to avoid backsliding.

---
