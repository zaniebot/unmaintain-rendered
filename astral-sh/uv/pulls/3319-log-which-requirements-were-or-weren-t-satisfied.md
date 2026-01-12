```yaml
number: 3319
title: "Log which requirements were or weren't satisfied"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/log-satisfies
created_at: 2024-04-30T09:05:13Z
updated_at: 2024-04-30T15:39:42Z
url: https://github.com/astral-sh/uv/pull/3319
synced_at: 2026-01-12T16:05:34Z
```

# Log which requirements were or weren't satisfied

---

_@konstin_

Previously, a noop `uv pip install` would only show "Audited {} package(s)" but no details, not even with `-vv`. Now it debug logs which requirements were met and it also debug logs which requirement was missing to trigger the full routine, allowing it investigate caching behaviour.

First `uv pip install -v jupyter`:

```
DEBUG At least one requirement is not satisfied: jupyter
```

Second `uv pip install -v jupyter`:

```
DEBUG Found a virtualenv named .venv at: /home/konsti/projects/uv-main/.venv
DEBUG Cached interpreter info for Python 3.12.1, skipping probing: .venv/bin/python
DEBUG Using Python 3.12.1 environment at .venv/bin/python
DEBUG Trying to lock if free: .venv/.lock
DEBUG Requirement satisfied: anyio
DEBUG Requirement satisfied: anyio>=3.1.0
DEBUG Requirement satisfied: argon2-cffi-bindings
DEBUG Requirement satisfied: argon2-cffi>=21.1
DEBUG Requirement satisfied: arrow>=0.15.0
DEBUG Requirement satisfied: asttokens>=2.1.0
DEBUG Requirement satisfied: async-lru>=1.0.0
DEBUG Requirement satisfied: attrs>=22.2.0
DEBUG Requirement satisfied: babel>=2.10
...
DEBUG Requirement satisfied: webencodings
DEBUG Requirement satisfied: webencodings>=0.4
DEBUG Requirement satisfied: websocket-client>=1.7
DEBUG Requirement satisfied: widgetsnbextension~=4.0.10
DEBUG All editables satisfied: 
Audited 1 package in 12ms
```

This will clash with the `tool.uv.sources` PR, i'll rebase it on top.


---

_Label `enhancement` added by @konstin on 2024-04-30 09:05_

---

_@zanieb reviewed on 2024-04-30 14:58_

---

_Review comment by @zanieb on `crates/uv-installer/src/site_packages.rs`:513 on 2024-04-30 14:58_

Should this derive debug and such?

---

_@zanieb reviewed on 2024-04-30 14:59_

---

_Review comment by @zanieb on `crates/uv-installer/src/site_packages.rs`:521 on 2024-04-30 14:59_

Can you add a comment as to why we only have one item here?

---

_@zanieb reviewed on 2024-04-30 15:00_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_install.rs`:189 on 2024-04-30 15:00_

Can we make these ones trace?

---

_Review comment by @zanieb on `crates/uv-installer/src/site_packages.rs`:296 on 2024-04-30 15:00_

This comment looks wrong

---

_@zanieb reviewed on 2024-04-30 15:00_

---

_@zanieb approved on 2024-04-30 15:00_

---

_@zanieb reviewed on 2024-04-30 15:12_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_install.rs`:189 on 2024-04-30 15:12_

I guess I'm actually okay with debug.

---

_@konstin reviewed on 2024-04-30 15:30_

---

_Review comment by @konstin on `crates/uv-installer/src/site_packages.rs`:513 on 2024-04-30 15:30_

Added a debug derive and a docstring

---

_@konstin reviewed on 2024-04-30 15:31_

---

_Review comment by @konstin on `crates/uv/src/commands/pip_install.rs`:189 on 2024-04-30 15:31_

These should stay debug messages, they are similar to the message about the packages that we installed. They are the nearly final messages (this is the early exist path), they are only followed by:

```
DEBUG All editables satisfied: [...]
Audited 1 package in 12ms
```

---

_@konstin reviewed on 2024-04-30 15:31_

---

_Review comment by @konstin on `crates/uv-installer/src/site_packages.rs`:296 on 2024-04-30 15:31_

good catch

---

_Merged by @konstin on 2024-04-30 15:39_

---

_Closed by @konstin on 2024-04-30 15:39_

---

_Branch deleted on 2024-04-30 15:39_

---
