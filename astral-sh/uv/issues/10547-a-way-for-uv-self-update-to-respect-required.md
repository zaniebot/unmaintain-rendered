```yaml
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
synced_at: 2026-01-12T16:00:15Z
```

# A way for `uv self update` to respect `required-version`

---

_@nathanjmcdougall_

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
