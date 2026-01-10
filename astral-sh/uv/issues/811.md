```yaml
number: 811
title: Hint that prereleases are available when dependency cannot be satisfied
type: issue
state: closed
author: zanieb
labels:
  - wish
  - error messages
assignees: []
created_at: 2024-01-05T23:11:29Z
updated_at: 2024-01-09T14:58:40Z
url: https://github.com/astral-sh/uv/issues/811
synced_at: 2026-01-10T05:40:31Z
```

# Hint that prereleases are available when dependency cannot be satisfied

---

_Issue opened by @zanieb on 2024-01-05 23:11_

e.g. in the following scenario

https://github.com/astral-sh/puffin/blob/88adba83a01357dec22bc7e2a0d79ab53e589d5c/crates/puffin-cli/tests/pip_install_scenarios.rs#L481-L494

we say no versions are available to satisfy the direct dependency

https://github.com/astral-sh/puffin/blob/88adba83a01357dec22bc7e2a0d79ab53e589d5c/crates/puffin-cli/tests/pip_install_scenarios.rs#L523-L524

but we could hint that the prerelease is available.

Similarly, in 

https://github.com/astral-sh/puffin/blob/88adba83a01357dec22bc7e2a0d79ab53e589d5c/crates/puffin-cli/tests/pip_install_scenarios.rs#L1067-L1084

we could hint that a prerelease is available to satisfy the transitive dependency. I'm less sure if we _should_ in that case.

---

_Label `error messages` added by @zanieb on 2024-01-05 23:11_

---

_Comment by @zanieb on 2024-01-08 15:34_

@charliermarsh feasible? desirable?

---

_Comment by @charliermarsh on 2024-01-08 18:00_

@zanieb - I think it's feasible... It's kind of "expensive" but feasible: check if we found no versions in a range, and then check if there are pre-releases for that package that would satisfy it. It seems like a good idea, probably...

---

_Label `wish` added by @zanieb on 2024-01-08 18:16_

---

_Comment by @zanieb on 2024-01-08 18:16_

Marking this as lower priority than the rest of the changes. I'd like to do it though.

---

_Comment by @charliermarsh on 2024-01-08 18:17_

I can prob do this one, it's in my wheelhouse.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-09 03:02_

---

_Closed by @charliermarsh on 2024-01-09 14:58_

---
