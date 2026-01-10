---
number: 13705
title: "Upgrading only one group packages, i.e., `--upgrade-group`"
type: issue
state: open
author: hovnatan
labels:
  - enhancement
assignees: []
created_at: 2025-05-28T19:43:28Z
updated_at: 2025-12-19T19:20:08Z
url: https://github.com/astral-sh/uv/issues/13705
synced_at: 2026-01-10T01:25:37Z
---

# Upgrading only one group packages, i.e., `--upgrade-group`

---

_Issue opened by @hovnatan on 2025-05-28 19:43_

### Question

I have a virtual environment  setup from multiple groups in dependencies (`default-groups="all"`). Now working in that virtual environment  I want to upgrade and sync lock file for packages only from one of the groups, e.g., something like this `uv sync --upgrade --group lint`, but that command upgrades all packages from all groups. Is it possible to achieve this behavior? If I try `uv sync --upgrade --only-group lint` that will remove packages from all other groups, which is not desired.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @hovnatan on 2025-05-28 19:43_

---

_Comment by @zanieb on 2025-05-28 19:47_

We don't have `--upgrade-group` semantics yet, unfortunately.

Can you share more about the use-case for only upgrading the single group?

cc @konstin for upgrade design work.

---

_Comment by @hovnatan on 2025-05-28 19:51_

The environment is divided into groups since sometimes only part of environment is needed. But regularly I am working in the full multi-group environment and I want to sync and upgrade one group only, e.g., test if new linters are available from group lint.

---

_Referenced in [opendatahub-io/notebooks#1204](../../opendatahub-io/notebooks/pulls/1204.md) on 2025-06-26 17:52_

---

_Comment by @insspb on 2025-06-28 03:02_

@zanieb This feature is highly required for projects that use uv as project dependency manager, not as manager for package release. There are common to use poetry with separation for groups like:
main, benchmarking, autotesting, linting, docs
And so highly common when only person/developer who responsible for benchmarking update own tools, same for docs and linting.

Also groups allow to separate packages on safe-to blind update and "require verification". 
In example above updating groups like autotesting is sage.
linting can be frozen for long time due corparate internal styleguides freezes (for example if somebody use black v 23 somethin)
docs is responsible for dedicated team, who can verify that everything compiled correctly during own work.

So groups separation is highly required feature for big projects management.

---

_Label `question` removed by @zanieb on 2025-06-28 03:09_

---

_Label `enhancement` added by @zanieb on 2025-06-28 03:09_

---

_Renamed from "Upgrading only one group packages" to "Upgrading only one group packages, i.e., `--upgrade-group`" by @zanieb on 2025-06-28 03:09_

---

_Comment by @JockeTF on 2025-07-08 12:02_

Duplicate of #12848?

---

_Comment by @insspb on 2025-08-12 05:59_

Returned to it in a project, where it is important. As we are waiting for solution, currently it is possible to use such trick (if venv is clear from other dependencies)
```bash
uv sync --only-group dev -U --inexact --dry-run 2>&1 | grep '+' | awk -F'==' '{print $1}' | awk -F'+ ' '{print "-P " $2}' | xargs uv sync
```

---

_Comment by @edgarrmondragon on 2025-12-19 18:49_

Something like `uv lock --upgrade-group docs`  or `uv lock --upgrade-group dev` would indeed be useful.

Imagine a team wants to update their locked runtime and dev dependencies at different cadences, so this would allow that.

---

_Referenced in [astral-sh/uv#12848](../../astral-sh/uv/issues/12848.md) on 2025-12-19 18:57_

---

_Comment by @zanieb on 2025-12-19 18:57_

I'm not sure we can do this effectively since there can be shared transitive dependencies?

---

_Comment by @charliermarsh on 2025-12-19 19:03_

I think it would still make sense. It seems useful?

---

_Comment by @zanieb on 2025-12-19 19:17_

What would we do? Treat it as `--upgrade-package <name>` for each direct dependency of the dependency group? Or infer allowed upgrades for all the transitive dependencies of the group too?

---

_Comment by @zanieb on 2025-12-19 19:17_

(I agree it seems useful)

---

_Comment by @charliermarsh on 2025-12-19 19:20_

I was thinking the former, sugar over `--upgrade-package` for everything in the group.

---

_Referenced in [astral-sh/uv#17192](../../astral-sh/uv/pulls/17192.md) on 2025-12-19 21:22_

---
