```yaml
number: 10413
title: How do I stop uv from modifying config files
type: issue
state: closed
author: Pilgrim1379
labels:
  - question
assignees: []
created_at: 2025-01-08T22:17:37Z
updated_at: 2025-01-08T22:29:56Z
url: https://github.com/astral-sh/uv/issues/10413
synced_at: 2026-01-12T16:00:13Z
```

# How do I stop uv from modifying config files

---

_@Pilgrim1379_

Every time UV updates, it modifies my `bashrc` and `zshrc files with `$HOME/.local/share/../bin/env`. 

How can I stop this? I already have the settings in my config files in a way that works for me.

---

_Comment by @charliermarsh on 2025-01-08 22:28_

`INSTALLER_NO_MODIFY_PATH` (see: https://docs.astral.sh/uv/configuration/environment/)

---

_Label `question` added by @charliermarsh on 2025-01-08 22:29_

---

_Comment by @charliermarsh on 2025-01-08 22:29_

Or here: https://docs.astral.sh/uv/configuration/installer/#disabling-shell-modifications

---

_Closed by @charliermarsh on 2025-01-08 22:29_

---
