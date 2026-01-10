---
number: 8590
title: "Install dependency groups in the `uv pip` interface, i.e., `uv pip install --group`"
type: issue
state: closed
author: hynek
labels:
  - compatibility
assignees: []
created_at: 2024-10-26T11:58:43Z
updated_at: 2025-03-17T18:44:12Z
url: https://github.com/astral-sh/uv/issues/8590
synced_at: 2026-01-10T01:24:30Z
---

# Install dependency groups in the `uv pip` interface, i.e., `uv pip install --group`

---

_Issue opened by @hynek on 2024-10-26 11:58_

Hi,

I got really excited about PEP 735 but then I noticed it can't really be used with unlocked packages?

Like in https://github.com/hynek/svcs/pull/107, I would like to be able to do `uv pip install --group dev --editable .` (instead of `uv pip install -e .[dev]` although if you just allowed to pass dep groups like this, I'd be happy too)

This is completely orthogonal that many open-source packages can't use locking yet due to #7533 – some/many people just don't want to go that route.

Or am I missing something?

---

_Comment by @zanieb on 2024-10-26 15:00_

We didn't add it to the `uv pip` API because it's not available there yet and we want to match their interface

- https://github.com/pypa/pip/issues/12963

---

_Comment by @hynek on 2024-10-26 15:21_

Ah cool, that was my suspicion!

---

_Label `compatibility` added by @charliermarsh on 2024-10-26 16:46_

---

_Referenced in [posit-dev/posit-sdk-py#316](../../posit-dev/posit-sdk-py/pulls/316.md) on 2024-10-29 06:15_

---

_Referenced in [astral-sh/uv#8969](../../astral-sh/uv/issues/8969.md) on 2024-11-09 13:10_

---

_Referenced in [astral-sh/uv#9140](../../astral-sh/uv/issues/9140.md) on 2024-11-15 21:31_

---

_Referenced in [astral-sh/uv#9457](../../astral-sh/uv/issues/9457.md) on 2024-11-26 23:36_

---

_Renamed from "uv pip install --group" to "Install dependency groups in the `uv pip` interface, i.e., `uv pip install --group`" by @zanieb on 2024-11-26 23:45_

---

_Referenced in [cthoyt/cookiecutter-snekpack#32](../../cthoyt/cookiecutter-snekpack/pulls/32.md) on 2024-11-27 10:17_

---

_Referenced in [skfolio/skfolio#107](../../skfolio/skfolio/pulls/107.md) on 2024-12-22 21:18_

---

_Comment by @aretrace on 2024-12-23 03:04_

> # pypa/pip/pull/13065

🙏 patiently awaiting this 🙏

---

_Referenced in [astral-sh/uv#10287](../../astral-sh/uv/issues/10287.md) on 2025-01-03 15:29_

---

_Comment by @Hasnep on 2025-01-10 07:08_

In the meantime, I'm using

```shell
uv export --only-group=... | uv pip install --requirements=-
```

to install specific dependency groups.

---

_Referenced in [astral-sh/uv#10861](../../astral-sh/uv/pulls/10861.md) on 2025-01-22 14:55_

---

_Closed by @Gankra on 2025-01-23 13:47_

---

_Closed by @Gankra on 2025-01-23 13:47_

---

_Reopened by @zanieb on 2025-01-23 20:43_

---

_Assigned to @Gankra by @charliermarsh on 2025-02-16 19:03_

---

_Referenced in [astral-sh/uv#11686](../../astral-sh/uv/pulls/11686.md) on 2025-02-21 14:42_

---

_Closed by @Gankra on 2025-03-17 18:44_

---

_Closed by @Gankra on 2025-03-17 18:44_

---
