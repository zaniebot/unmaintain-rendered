```yaml
number: 2395
title: "Trim injected `python_version` marker to (major, minor)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/marker
created_at: 2024-03-12T21:16:04Z
updated_at: 2024-03-13T00:11:51Z
url: https://github.com/astral-sh/uv/pull/2395
synced_at: 2026-01-12T16:05:01Z
```

# Trim injected `python_version` marker to (major, minor)

---

_@charliermarsh_

## Summary

Per [PEP 508](https://peps.python.org/pep-0508/), `python_version` is just major and minor:

![Screenshot 2024-03-12 at 5 15 09 PM](https://github.com/astral-sh/uv/assets/1309177/cc3b8d65-dab3-4229-aed7-c6fe590b8da0)

Right now, we're using the provided version directly, so if it's, e.g., `-p 3.11.8`, we'll inject the wrong marker. This was causing `pandas` to omit `numpy` when `-p 3.11.8` was provided, since its markers look like:

```
Requires-Dist: numpy<2,>=1.22.4; python_version < "3.11"
Requires-Dist: numpy<2,>=1.23.2; python_version == "3.11"
Requires-Dist: numpy<2,>=1.26.0; python_version >= "3.12"
```

Closes https://github.com/astral-sh/uv/issues/2392.


---

_Review requested from @zanieb by @charliermarsh on 2024-03-12 21:16_

---

_Label `bug` added by @charliermarsh on 2024-03-12 21:16_

---

_@charliermarsh reviewed on 2024-03-12 21:16_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/python_version.rs`:39 on 2024-03-12 21:16_

I guess we can support this, but it makes the code below simpler because we can construct `markers.python_full_version` by _just_ looking at the releases. Otherwise, I need to add some more handling to it.

---

_Marked ready for review by @charliermarsh on 2024-03-12 21:17_

---

_@zanieb reviewed on 2024-03-12 21:19_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/python_version.rs`:81 on 2024-03-12 21:19_

Mildly confused that we use `.get(...).unwrap...` elsewhere but not here?

---

_@zanieb approved on 2024-03-12 21:19_

---

_@charliermarsh reviewed on 2024-03-12 23:44_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/python_requirement.rs`:17 on 2024-03-12 23:44_

This is the same value, I just renamed the method because it's the `python_full_version` marker.

---

_Merged by @charliermarsh on 2024-03-13 00:11_

---

_Closed by @charliermarsh on 2024-03-13 00:11_

---

_Branch deleted on 2024-03-13 00:11_

---
