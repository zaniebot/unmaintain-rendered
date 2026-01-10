---
number: 3602
title: "`uv pip compile --upgrade` can fail when `uv pip compile` succeeds if `requirements.txt` contains pre-release"
type: issue
state: open
author: Phlogistique
labels:
  - enhancement
assignees: []
created_at: 2024-05-15T08:15:01Z
updated_at: 2024-05-17T21:20:39Z
url: https://github.com/astral-sh/uv/issues/3602
synced_at: 2026-01-10T01:23:29Z
---

# `uv pip compile --upgrade` can fail when `uv pip compile` succeeds if `requirements.txt` contains pre-release

---

_Issue opened by @Phlogistique on 2024-05-15 08:15_

My `requirements.in` contains a package which depends on `betterproto>=2.0.0b6`. The `requirements.txt` was initially generated using `pip` and contains `betterproto==2.0.0b6`.

Running `uv pip compile` works, but `uv pip compile --upgrade` refuses to pick up that pre-release version, and therefore fails resolution.

I would think it would be more intuitive for either:
* `uv pip compile` to fail if any pre-release version is already in `requirements.txt`, or
* `uv pip compile --upgrade` to accept pre-releases if they are already in `requirements.txt`

---

_Comment by @charliermarsh on 2024-05-15 14:50_

I think "`uv pip compile --upgrade` to accept pre-releases if they are already in `requirements.txt`" does make sense.

---

_Label `enhancement` added by @charliermarsh on 2024-05-15 14:50_

---

_Comment by @charliermarsh on 2024-05-16 16:54_

I would probably say the same of yanked packages... If you run with `--upgrade`, and your lockfile uses a yanked package, we should probably allow you to continue using it...?

---

_Comment by @zanieb on 2024-05-17 02:03_

That's a little more dubious imo. If something is yanked you ought to be pushed to resolve it. If you want to keep using the yanked version you should have to pin? Can we consider that separately? (see #3644)

---

_Comment by @charliermarsh on 2024-05-17 13:59_

Yeah that's fine.

---

_Referenced in [astral-sh/uv#3644](../../astral-sh/uv/issues/3644.md) on 2024-05-17 18:39_

---

_Referenced in [astral-sh/uv#5729](../../astral-sh/uv/issues/5729.md) on 2024-08-02 15:25_

---
