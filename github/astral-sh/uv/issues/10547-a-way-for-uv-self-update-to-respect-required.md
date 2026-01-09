---
number: 10547
title: "A way for `uv self update` to respect `required-version`"
type: issue
state: open
author: nathanjmcdougall
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-01-12T20:45:34Z
updated_at: 2025-01-13T02:40:49Z
url: https://github.com/astral-sh/uv/issues/10547
synced_at: 2026-01-07T13:12:18-06:00
---

# A way for `uv self update` to respect `required-version`

---

_Issue opened by @nathanjmcdougall on 2025-01-12 20:45_

At the moment, with the following configuration:

```TOML
[tool.uv]
required-version = ">=0.5.0,<0.5.18"
```

Calling `uv self update` will default to installing the latest version (e.g. 0.5.18 at time of writing)., even though this may violate the `required-version` constraint.

I would like there to be an option to install the latest version _which satisfies_ `required-version`, e.g. in the above case to install 0.5.17. This would be useful for onboarding to a project, since the a script which calls `uv self update` wouldn't need to hard-code a version to ensure consistency between users' installed version.

---

_Label `enhancement` added by @charliermarsh on 2025-01-13 02:40_

---

_Label `wish` added by @charliermarsh on 2025-01-13 02:40_

---

_Referenced in [astral-sh/uv#12787](../../astral-sh/uv/issues/12787.md) on 2025-04-09 20:55_

---

_Referenced in [tonkintaylor/rastr#120](../../tonkintaylor/rastr/issues/120.md) on 2025-08-27 04:38_

---

_Referenced in [tonkintaylor/rastr#121](../../tonkintaylor/rastr/pulls/121.md) on 2025-08-27 04:39_

---

_Referenced in [astral-sh/uv#16014](../../astral-sh/uv/issues/16014.md) on 2025-10-10 11:50_

---
