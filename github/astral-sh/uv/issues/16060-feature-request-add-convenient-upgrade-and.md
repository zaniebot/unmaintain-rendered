---
number: 16060
title: "Feature Request: Add convenient upgrade and environment management features"
type: issue
state: open
author: sysrootinit
labels:
  - enhancement
assignees: []
created_at: 2025-09-29T03:18:39Z
updated_at: 2025-09-29T10:39:42Z
url: https://github.com/astral-sh/uv/issues/16060
synced_at: 2026-01-07T13:12:19-06:00
---

# Feature Request: Add convenient upgrade and environment management features

---

_Issue opened by @sysrootinit on 2025-09-29 03:18_

### Summary

uv is already an excellent tool, but some common developer workflows are still missing or incomplete. Below are several feature requests that would make uv more practical in everyday use.
	1.	One-click upgrade of all packages in a bare venv
	•	A command such as uv pip upgrade --all to upgrade every installed package in the current virtual environment.
	•	This is particularly useful for quick experiments or temporary environments.
	•	At the moment, this workflow is only possible in project mode (uv sync --upgrade), not in plain virtualenvs.
	2.	Respect existing pip configuration (pip.conf / PIP_INDEX_URL)
	•	Many users and organizations already configure custom indexes or mirrors via pip.conf or environment variables.
	•	uv currently does not read these settings. Supporting them would lower the barrier to adoption.
	•	Clearly documenting which pip settings are supported (and which are not) would also help.
	3.	Show Python version for installed tools
	•	uv tool list should display which Python interpreter each tool uses.
	•	This makes it easier to debug path and environment issues, especially when multiple Python versions are installed.
	4.	More intuitive editable installs
	•	Improve support for uv pip install -e . or uv add --editable.
	•	Developers frequently use editable installs for local development; behavior should be predictable and stable.
	5.	Better workspace / monorepo support
	•	Large projects often have multiple sub-modules.
	•	uv could provide stronger support for shared dependencies, lock file management, and cross-module sync in monorepo structures.
	•	Current behavior sometimes causes “module not found” or import issues in these setups.

---


### Example

_No response_

---

_Label `enhancement` added by @sysrootinit on 2025-09-29 03:18_

---

_Comment by @my1e5 on 2025-09-29 10:39_

You may find it helpful to check out [Before posting in the issue tracker](https://github.com/astral-sh/uv/issues/9452), since a number of the points you’ve raised are already covered by popular feature requests or existing functionality

1. https://github.com/astral-sh/uv/issues/1419
2. https://github.com/astral-sh/uv/issues/1404
3. https://github.com/astral-sh/uv/pull/15814
4. https://docs.astral.sh/uv/pip/packages/#editable-packages (Editable installs are already supported)
5. https://docs.astral.sh/uv/concepts/projects/workspaces (Workspaces are also already supported)

For 4 and 5, if there is specific functionality you think needs improving, it would be best to open a focused issue which covers your points.

---
