---
number: 16527
title: Unexpected dependency upgrades when downgrading a single package with uv add
type: issue
state: open
author: JuanCruzC97
labels:
  - question
assignees: []
created_at: 2025-10-30T22:45:59Z
updated_at: 2025-12-18T15:02:04Z
url: https://github.com/astral-sh/uv/issues/16527
synced_at: 2026-01-10T01:26:07Z
---

# Unexpected dependency upgrades when downgrading a single package with uv add

---

_Issue opened by @JuanCruzC97 on 2025-10-30 22:45_

### Question

I ran into a situation where downgrading one dependency caused many other packages to update unexpectedly.

My project depends tightly on the darts library. I initially added it using:

uv add u8darts[all]


This created a requirement like u8darts[all]>=0.37.1 in my pyproject.toml.

At some point, darts was updated to version 0.38.0 (not sure when it happened), which introduced breaking changes. To fix this, I tried downgrading with:

uv add u8darts[all]==0.37.1

However, uv then upgraded many other dependencies in the environment, even though I was only trying to downgrade this one package.

<img width="418" height="904" alt="Image" src="https://github.com/user-attachments/assets/448c6f9e-4c4c-4455-94e3-d0113279704f" />

Questions:

Why does uv trigger many package upgrades when downgrading a single dependency? Probably this is the cause for darts being upgraded to 0.38.0 in the first place. How can I avoid this?

What’s the recommended workflow for pinning or downgrading a dependency like this without affecting unrelated packages?

What am I missing or doing wrong here? What’s the best setup or workflow for this kind of use case?

### Platform

Windows 11

### Version

uv 0.6.11 (0632e24d1 2025-03-30)

---

_Label `question` added by @JuanCruzC97 on 2025-10-30 22:46_

---

_Comment by @muhammad-fiaz on 2025-10-31 06:21_

@JuanCruzC97  have u tried isolation or dependencies constraints settings from `uv` you can refer [here](https://docs.astral.sh/uv/reference/settings/)  and also try to update `uv` version to `latest` by `uv self update`

---

_Comment by @konstin on 2025-12-18 15:02_

This should happen very rarely, and only when there was a conflict resolving dependencies, in a way that uv couldn't resolve when starting with the versions in the lockfile. I recommend putting upper bounds on dependencies which tend to make breaking changes as protection, otherwise uv can't really know that it must not upgrade.

---
