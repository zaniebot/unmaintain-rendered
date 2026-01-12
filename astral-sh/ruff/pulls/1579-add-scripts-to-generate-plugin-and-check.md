```yaml
number: 1579
title: Add scripts to generate plugin and check boilerplate
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/scripts
created_at: 2023-01-03T02:44:28Z
updated_at: 2023-01-03T03:11:14Z
url: https://github.com/astral-sh/ruff/pull/1579
synced_at: 2026-01-12T05:36:31Z
```

# Add scripts to generate plugin and check boilerplate

---

_Pull request opened by @charliermarsh on 2023-01-03 02:44_

These are pretty hacky, but they work, and they'll save us a ton of time. Since they're dev-only, they can be pretty scrappy, so let's just iterate on them as we go.

To generate a plugin:

```shell
python scripts/add_plugin.py flake8-pie --url https://pypi.org/project/flake8-pie/0.16.0/
```

To generate a check code:

```shell
python scripts/add_check.py --name PreferListBuiltin --code PIE807 --plugin flake8-pie
```

Resolves #1531 and #1532.


---

_@not-my-profile reviewed on 2023-01-03 03:06_

---

_Review comment by @not-my-profile on `scripts/add_check.py`:117 on 2023-01-03 03:06_

Since these arguments are required, I think I'd rather make them positional e.g.

```
python scripts/add_check.py flake8-pie PIE807 PreferListBuiltin
```

---

_@charliermarsh reviewed on 2023-01-03 03:07_

---

_Review comment by @charliermarsh on `scripts/add_check.py`:117 on 2023-01-03 03:07_

My gripe with that is that you then have to remember the order.

---

_Merged by @charliermarsh on 2023-01-03 03:10_

---

_Closed by @charliermarsh on 2023-01-03 03:10_

---

_Branch deleted on 2023-01-03 03:10_

---

_@not-my-profile reviewed on 2023-01-03 03:11_

---

_Review comment by @not-my-profile on `scripts/add_check.py`:117 on 2023-01-03 03:11_

Right, I think that could be mitigated by doing some input validation and providing nice error messages e.g:

* providing a nice error message if the first argument isn't an existing plugin
* providing a nice error message if the second argument doesn't have the prefix of the plugin

---
