```yaml
number: 675
title: "Add editable install support to `pip-install`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/install
created_at: 2023-12-17T19:01:43Z
updated_at: 2023-12-18T08:52:34Z
url: https://github.com/astral-sh/uv/pull/675
synced_at: 2026-01-10T15:44:44Z
```

# Add editable install support to `pip-install`

---

_Pull request opened by @charliermarsh on 2023-12-17 19:01_

Per the title: adds support for `-e` installs to `puffin pip-install`. There were some challenges here around threading the editable installs to the right places. Namely, we want to build _once_, then reuse the editable installs from the resolution. At present, we were losing the `editable: true` flag on the `Dist` that came back through the resolution, so it required some changes to the resolver.

Closes https://github.com/astral-sh/puffin/issues/672.

---

_Review requested from @konstin by @charliermarsh on 2023-12-17 19:01_

---

_Label `enhancement` added by @charliermarsh on 2023-12-17 19:01_

---

_@charliermarsh reviewed on 2023-12-17 19:07_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_install.rs`:395 on 2023-12-17 19:07_

One weird thing here: if the resolution is fully satisfied by the venv, we abort. However, if it's not, we rebuild _all_ the editables, to power the resolution. Then, we come here to do the install plan, which will mark any editables that need to be installed as `editables`. So, in short: we build all editables, but if some are already installed in the venv, we only _install_ those that are missing.

---

_@charliermarsh reviewed on 2023-12-17 19:09_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_install.rs`:395 on 2023-12-17 19:09_

I guess I'll change this to always re-install.

---

_@charliermarsh reviewed on 2023-12-17 19:14_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_install.rs`:395 on 2023-12-17 19:14_

Alright, we now _always_ reinstall the editables if we bothered to build them.

---

_@charliermarsh reviewed on 2023-12-18 02:24_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/resolution.rs`:87 on 2023-12-18 02:24_

(Removed in the next PR anyway.)

---

_@charliermarsh reviewed on 2023-12-18 02:24_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/plan.rs`:374 on 2023-12-18 02:24_

(Removed in the next PR anyway.)

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_install.rs`:517 on 2023-12-18 08:50_

Here we're still missing the installed-as-editable mapping

---

_@konstin approved on 2023-12-18 08:52_

---

_Merged by @konstin on 2023-12-18 08:52_

---

_Closed by @konstin on 2023-12-18 08:52_

---

_Branch deleted on 2023-12-18 08:52_

---
