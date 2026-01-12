```yaml
number: 14131
title: "`uv upgrade` and `uv venv --preview` no longer create symlink directories"
type: pull_request
state: merged
author: jtfmumm
labels:
  - enhancement
assignees: []
merged: true
base: feature/transparent-python-upgrades
head: jtfm/upgrade-doesnt-create-symlink
created_at: 2025-06-18T16:01:41Z
updated_at: 2025-06-19T07:27:55Z
url: https://github.com/astral-sh/uv/pull/14131
synced_at: 2026-01-12T16:11:02Z
```

# `uv upgrade` and `uv venv --preview` no longer create symlink directories

---

_@jtfmumm_

For the preview version of transparent upgrades, this PR helps to more clearly distinguish two features: (1) upgrading by installing the latest patch version for a minor version and (2) supporting transparent upgrades for things like virtual environments. 

Transparently upgradeable installations now require `uv python install` with the `--preview` flag. `uv python upgrade` will no longer create symlink directories on its own, only point one to the latest patch if it already exists. 

This PR also changes `uv venv --preview` to no longer create a missing symlink directory (a behavior that was meant to support a smooth transition when upgrading uv, but which doesn't make sense in light of the core change of this PR).


---

_Label `enhancement` added by @jtfmumm on 2025-06-18 16:01_

---

_Label `do-not-merge` added by @jtfmumm on 2025-06-18 16:35_

---

_Label `do-not-merge` removed by @jtfmumm on 2025-06-18 17:41_

---

_Review requested from @zanieb by @jtfmumm on 2025-06-18 17:42_

---

_@zanieb approved on 2025-06-18 21:29_

---

_Comment by @zanieb on 2025-06-18 21:36_

Thanks! Appreciate that you caught the `venv` case too.

---

_Merged by @jtfmumm on 2025-06-19 07:27_

---

_Closed by @jtfmumm on 2025-06-19 07:27_

---

_Branch deleted on 2025-06-19 07:27_

---
