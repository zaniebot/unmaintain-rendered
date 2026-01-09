---
number: 10686
title: "pip-compatiblity: PIP_CONSTRAINT and PIP_CONSTRAINTS are not used, only UV_CONSTRAINT is used"
type: issue
state: open
author: ssbarnea
labels: []
assignees: []
created_at: 2025-01-16T17:13:50Z
updated_at: 2025-01-16T22:49:56Z
url: https://github.com/astral-sh/uv/issues/10686
synced_at: 2026-01-07T13:12:18-06:00
---

# pip-compatiblity: PIP_CONSTRAINT and PIP_CONSTRAINTS are not used, only UV_CONSTRAINT is used

---

_Issue opened by @ssbarnea on 2025-01-16 17:13_

I found the hard way after migrating from pip to uv on few projects that my lock files are no longer working, just because `uv pip` command seems to ignore presence of `PIP_CONSTRAINT` or `PIP_CONSTRAINTS` file and thus failed to lock the dependencies during install.

I tried defining `UV_CONSTRAINT` and it worked as expected. This is a divergence from the claimed API compatibility. Sadly the documentation page from https://docs.astral.sh/uv/pip/compatibility/#build-constraints does mention the constraints only on build constraints, not also on install.

I do believe that the desirable fix here is to look for environment variables in this order:

1. UV_CONSTRAINT
2. PIP_CONSTRAINTS
3. PIP_CONSTRAINT (quile uncommon name but apparently supported by pip)


## Example

```
❯ UV_CONSTRAINT=.config/constraints.txt uv pip install .[docs] --dry-run
Using Python 3.13.1 environment at: .tox/docs
Resolved 75 packages in 17ms
Would download 1 package
Would uninstall 2 packages
Would install 2 packages
 - ansible-compat==25.0.1.dev0 (from file:///Users/ssbarnea/code/a/ansible-compat/.tox/.tmp/package/22/ansible_compat-25.0.1.dev0-0.editable-py3-none-any.whl)
 + ansible-compat @ file:///Users/ssbarnea/code/a/ansible-compat
 - mkdocs-autorefs==1.3.0
 + mkdocs-autorefs==1.2.0
```

```
❯ PIP_CONSTRAINT=.config/constraints.txt uv pip install .[docs] --dry-run
Using Python 3.13.1 environment at: .tox/docs
Resolved 75 packages in 15ms
Would download 1 package
Would uninstall 1 package
Would install 1 package
 - ansible-compat==25.0.1.dev0 (from file:///Users/ssbarnea/code/a/ansible-compat/.tox/.tmp/package/22/ansible_compat-25.0.1.dev0-0.editable-py3-none-any.whl)
 + ansible-compat @ file:///Users/ssbarnea/code/a/ansible-compat
```

---

_Comment by @charliermarsh on 2025-01-16 17:16_

We don’t respect pip environment variables anywhere, as a rule. The full list of environment variables that affect uv is enumerated here: https://docs.astral.sh/uv/configuration/environment/

---

_Referenced in [ansible/ansible-compat#446](../../ansible/ansible-compat/pulls/446.md) on 2025-01-16 17:17_

---

_Referenced in [tox-dev/tox-uv#154](../../tox-dev/tox-uv/pulls/154.md) on 2025-01-16 17:35_

---

_Comment by @ssbarnea on 2025-01-16 22:49_

@charliermarsh Maybe it would worth displaying a warning if PIP_CONSTRAINT(S) is found as defined, mentioning the uv will not use it. This could save a good number of hours to those trying to migrate or use both.

As someone that defined these in his user profile, I do face few surprises...
```
pip='python3 -m uv pip'
pip3='python3 -m uv pip'
```

---

_Referenced in [ansible/molecule#4367](../../ansible/molecule/pulls/4367.md) on 2025-01-20 16:38_

---
