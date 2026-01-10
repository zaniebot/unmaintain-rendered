```yaml
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
synced_at: 2026-01-10T03:50:30Z
```

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

_Renamed from "uv pip install --group" to "Install dependency groups in the `uv pip` interface, i.e., `uv pip install --group`" by @zanieb on 2024-11-26 23:45_

---

_Comment by @aretrace on 2024-12-23 03:04_

> # pypa/pip/pull/13065

🙏 patiently awaiting this 🙏

---

_Comment by @Hasnep on 2025-01-10 07:08_

In the meantime, I'm using

```shell
uv export --only-group=... | uv pip install --requirements=-
```

to install specific dependency groups.

---

_Closed by @Gankra on 2025-01-23 13:47_

---

_Closed by @Gankra on 2025-01-23 13:47_

---

_Reopened by @zanieb on 2025-01-23 20:43_

---

_Assigned to @Gankra by @charliermarsh on 2025-02-16 19:03_

---

_Closed by @Gankra on 2025-03-17 18:44_

---

_Closed by @Gankra on 2025-03-17 18:44_

---
