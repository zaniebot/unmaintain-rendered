```yaml
number: 11000
title: Update documentation for activating virtual environments in different shell
type: pull_request
state: merged
author: CedricRaison
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/add-shell-activations
created_at: 2025-01-27T18:58:17Z
updated_at: 2025-01-27T19:24:48Z
url: https://github.com/astral-sh/uv/pull/11000
synced_at: 2026-01-10T11:45:22Z
```

# Update documentation for activating virtual environments in different shell

---

_Pull request opened by @CedricRaison on 2025-01-27 18:58_

## Add activation commands for fish shell and other alternative shells

While trying to use uv with fish shell, I encountered an issue as `source .venv/bin/activate` didn't work. The documentation didn't specify that fish shell requires using `source .venv/bin/activate.fish` instead. I created issue #10986 to address this.

This PR improves the documentation by:
- Adding the correct activation command for fish shell: `source .venv/bin/activate.fish`
- Adding the correct activation command for Nushell: `use .venv\Scripts\activate.nu`
- Adding the correct activation command for Tcsh: `use .venv/bin/activate.csh`

This will help users of alternative shells to properly activate their virtual environments without encountering the same confusion I experienced.

Fixes #10986

---

_Renamed from "Update documentation for activating virtual environments in different…" to "Update documentation for activating virtual environments in different shell" by @CedricRaison on 2025-01-27 18:58_

---

_Label `documentation` added by @zanieb on 2025-01-27 19:24_

---

_@zanieb approved on 2025-01-27 19:24_

Thanks! I pushed a commit that restructures this a bit.

---

_Merged by @zanieb on 2025-01-27 19:24_

---

_Closed by @zanieb on 2025-01-27 19:24_

---
