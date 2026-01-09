---
number: 12013
title: Configure dependency sources only locally
type: issue
state: open
author: shfattig
labels:
  - enhancement
assignees: []
created_at: 2025-03-06T18:06:30Z
updated_at: 2025-03-06T19:14:40Z
url: https://github.com/astral-sh/uv/issues/12013
synced_at: 2026-01-07T13:12:18-06:00
---

# Configure dependency sources only locally

---

_Issue opened by @shfattig on 2025-03-06 18:06_

### Summary

When working on multiple dependent repos at once, a developer may want to configure uv to install some dependencies from a file location. This works great for local development, but on a shared branch his paths may be different from another developer's - or further, other developers may not even share the same need to install those packages from source locally. It would be very helpful to enable some setup such as a uv.toml override that can be kept locally and .gitignored to maintain a local set of sources different from the configuration checked into git.

### Example

A developer is working on package A. He is also simultaneously developing package A.1 and A.2 which are dependencies of package A. He would like to configure his package A repo to install from source at the file location of A.1 and A.2 source code, but knows his peer A developers (whom are _not_ developing A.1 and A.2) would be affected by a change to the shared pyproject.toml `tool.uv.sources` section

---

_Label `enhancement` added by @shfattig on 2025-03-06 18:06_

---

_Comment by @zanieb on 2025-03-06 19:14_

I think this is loosely a duplicate of https://github.com/astral-sh/uv/issues/6772

---

_Referenced in [astral-sh/uv#9258](../../astral-sh/uv/issues/9258.md) on 2025-04-23 17:14_

---

_Referenced in [astral-sh/uv#13073](../../astral-sh/uv/issues/13073.md) on 2025-04-23 19:27_

---
