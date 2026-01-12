```yaml
number: 14084
title: "Warn when `uv python upgrade` tries to install over a non-managed `bin` installation"
type: pull_request
state: merged
author: jtfmumm
labels:
  - enhancement
assignees: []
merged: true
base: feature/transparent-python-upgrades
head: jtfm/restore-force-to-upgrade
created_at: 2025-06-16T19:15:42Z
updated_at: 2025-06-18T14:39:09Z
url: https://github.com/astral-sh/uv/pull/14084
synced_at: 2026-01-12T16:11:01Z
```

# Warn when `uv python upgrade` tries to install over a non-managed `bin` installation

---

_@jtfmumm_

`uv python upgrade` had originally had a `--force` option, but we thought it probably didn't make sense. However, switching `uv python upgrade` behind the preview flag revealed a bad interaction: upgrading a minor version when a non-managed interpreter of that version exists in the `bin` directory displays an error recommending the use of `--force`.

This PR only warns if this case arises with `uv python upgrade`, noting that install `--force` can be used. It also includes a test for the case described above. 

---

_Review requested from @konstin by @jtfmumm on 2025-06-16 19:15_

---

_Comment by @zanieb on 2025-06-16 19:18_

Should we hint / require `uv python install --force` instead? I worry about the semantics of `uv python upgrade --force`, it seems less obvious.


Shouldn't `uv python upgrade` just continue if it encounters a Python in the `bin/` it does not manage? Perhaps with a warning?

---

_Label `enhancement` added by @jtfmumm on 2025-06-17 12:36_

---

_Renamed from "Restore the `--force` option for `uv python upgrade`" to "Warn when `uv python upgrade` tries to install over a non-managed `bin` installation" by @jtfmumm on 2025-06-17 14:14_

---

_Comment by @jtfmumm on 2025-06-17 14:28_

> Should we hint / require `uv python install --force` instead? I worry about the semantics of `uv python upgrade --force`, it seems less obvious.
> 
> Shouldn't `uv python upgrade` just continue if it encounters a Python in the `bin/` it does not manage? Perhaps with a warning?

I ended up removing the `upgrade --force` option. In the test case, upgrade continues with a warning, hinting to use force install if you want to install over the non-managed `bin` installation.

I've updated the PR title and description.

---

_Review requested from @zanieb by @jtfmumm on 2025-06-18 14:01_

---

_@zanieb approved on 2025-06-18 14:35_

---

_Merged by @jtfmumm on 2025-06-18 14:39_

---

_Closed by @jtfmumm on 2025-06-18 14:39_

---

_Branch deleted on 2025-06-18 14:39_

---
