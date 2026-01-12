```yaml
number: 754
title: Add an up-to-date README with Puffin documentation
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/docs
created_at: 2024-01-03T21:11:24Z
updated_at: 2024-01-04T02:22:02Z
url: https://github.com/astral-sh/uv/pull/754
synced_at: 2026-01-12T16:04:10Z
```

# Add an up-to-date README with Puffin documentation

---

_@charliermarsh_

_No description provided._

---

_Review requested from @zanieb by @charliermarsh on 2024-01-03 21:11_

---

_Review requested from @konstin by @charliermarsh on 2024-01-03 21:11_

---

_Label `documentation` added by @charliermarsh on 2024-01-03 21:11_

---

_Review comment by @konstin on `README.md`:11 on 2024-01-03 21:20_

caveat, the copy-on-write is very platform dependent. ext4 or ntfs user hoping for reflinks will be disappointed 

---

_Review comment by @konstin on `README.md`:13 on 2024-01-03 21:21_

Tbh i'd make parity with pip a non-goal, there's too much strange histories buried in its api

---

_Review comment by @konstin on `README.md`:17 on 2024-01-03 21:23_

Dependency-less, single static binary! (plus minus git support, details to be checked)

---

_Review comment by @konstin on `README.md`:21 on 2024-01-03 21:25_

I'd put pipx and cargo-dist first, for the global installation, pip requires you already have the venv.

---

_Review comment by @konstin on `README.md`:65 on 2024-01-03 21:27_

```suggestion
Our goal is "Cargo for Python": a Python package manager that is extremely fast, reliable,
```


---

_Review comment by @konstin on `README.md`:46 on 2024-01-03 21:28_

I'd advertise pyproject.toml here already, it's what we want to nudge users towards

---

_Review comment by @konstin on `README.md`:74 on 2024-01-03 21:55_

How much of an upgrade path from requirements.{in,txt} to puffin pyproject.toml with proper lockfile do we want to provide?

---

_Comment by @zanieb on 2024-01-03 21:57_

[Rendered](https://github.com/astral-sh/puffin/blob/charlie/docs/README.md)

---

_Review comment by @konstin on `README.md`:90 on 2024-01-03 21:58_

I'm not sure if i want to put them in the same list, we don't want to support eggs (they are ancient and dying out) but do very much want to support hash checking (it's a requirement for more security focussed use cases).

---

_Review comment by @konstin on `README.md`:106 on 2024-01-03 21:58_

```suggestion
You can switch to
```


---

_Review comment by @konstin on `README.md`:165 on 2024-01-03 22:00_

```suggestion
For local path dependencies, Puffin caches based on the last modified time of the `setup.py` or
```


---

_Review comment by @konstin on `README.md`:168 on 2024-01-03 22:01_

I'd put the clean command last, given that it should be the last resort.

---

_Review comment by @konstin on `README.md`:177 on 2024-01-03 22:01_

I'd switch the order

---

_Review comment by @charliermarsh on `README.md`:9 on 2024-01-03 22:03_

This is just a placeholder, we need an actual benchmark for the top of the README.

---

_@charliermarsh reviewed on 2024-01-03 22:03_

---

_Review comment by @konstin on `README.md`:188 on 2024-01-03 22:03_

I think I'd also want a pyproject.toml table for a list of allowed prerelease package names.

---

_@charliermarsh reviewed on 2024-01-03 22:04_

---

_Review comment by @charliermarsh on `README.md`:98 on 2024-01-03 22:04_

Maybe each of these should be collapsed?

---

_Review comment by @konstin on `README.md`:205 on 2024-01-03 22:04_

I'd gate this to `>=2.0,<3`

---

_@zanieb reviewed on 2024-01-03 22:05_

---

_Review comment by @zanieb on `README.md`:19 on 2024-01-03 22:05_

Should we link to a gist of the packages? Aren't we doing 10k too?

---

_@konstin approved on 2024-01-03 22:06_

---

_Review comment by @zanieb on `README.md`:64 on 2024-01-03 22:07_

Seems unnecessarily wordy
```suggestion
Puffin is not a complete "package manager". Instead, it represents an intermediary goal
```

---

_@zanieb reviewed on 2024-01-03 22:07_

---

_@konstin reviewed on 2024-01-03 22:16_

---

_Review comment by @konstin on `README.md`:19 on 2024-01-03 22:16_

I don't think I'd make that claim, because there is still a good bunch of failures in the top 10k set, e.g. around prereleases and some that simply don't support my os/python version. I also haven't run with builds in a while, we might find some more there.

---

_@charliermarsh reviewed on 2024-01-03 22:32_

---

_Review comment by @charliermarsh on `README.md`:19 on 2024-01-03 22:32_

Can we say something less bold, like "Tested against the top 10,000 packages"?

---

_@konstin reviewed on 2024-01-03 22:40_

---

_Review comment by @konstin on `README.md`:19 on 2024-01-03 22:40_

:+1:

---

_@charliermarsh reviewed on 2024-01-04 02:06_

---

_Review comment by @charliermarsh on `README.md`:74 on 2024-01-04 02:06_

Some for sure.

---

_Merged by @charliermarsh on 2024-01-04 02:22_

---

_Closed by @charliermarsh on 2024-01-04 02:22_

---

_Branch deleted on 2024-01-04 02:22_

---
