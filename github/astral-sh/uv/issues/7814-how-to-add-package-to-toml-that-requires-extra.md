---
number: 7814
title: How to add package to toml that requires extra scripts to install
type: issue
state: open
author: itrase
labels:
  - wish
assignees: []
created_at: 2024-09-30T16:06:17Z
updated_at: 2024-10-04T13:22:31Z
url: https://github.com/astral-sh/uv/issues/7814
synced_at: 2026-01-07T13:12:17-06:00
---

# How to add package to toml that requires extra scripts to install

---

_Issue opened by @itrase on 2024-09-30 16:06_

Apologies if I'm not phrasing this correctly. The [PyRosetta](https://www.pyrosetta.org/downloads) package has a very strange set of install instructions. Specifically, they are:

```
pip install pyrosetta-installer 
python -c 'import pyrosetta_installer; pyrosetta_installer.install_pyrosetta()'
```

I'm not sure how to translate the second line to work in a toml file. Is there a simple way to do this, perhaps in the uv.tool section?

---

_Label `wish` added by @konstin on 2024-10-04 13:19_

---

_Comment by @konstin on 2024-10-04 13:19_

pyrossetta is internally calling `subprocess.check_call(f'pip install {url}', shell=True)`, I'm afraid we don't support that pattern of installation currently in `pyproject.toml`; We also currently don't support post-install commands.

---

_Comment by @itrase on 2024-10-04 13:22_

That makes sense - thanks for taking the time to look into this!

---
