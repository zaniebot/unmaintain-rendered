```yaml
number: 16318
title: "fix: Add a hint on `uv pip install` failure if the `--system` flag is used to select an externally managed interpreter"
type: pull_request
state: merged
author: cmnemoi
labels:
  - error messages
assignees: []
merged: true
base: main
head: fix/15639
created_at: 2025-10-15T16:37:35Z
updated_at: 2025-10-21T07:46:40Z
url: https://github.com/astral-sh/uv/pull/16318
synced_at: 2026-01-10T06:36:16Z
```

# fix: Add a hint on `uv pip install` failure if the `--system` flag is used to select an externally managed interpreter

---

_Pull request opened by @cmnemoi on 2025-10-15 16:37_

Hello,

# Summary
This PR makes the error message clearer when you try to install packages into an externally managed Python environment with the `--system` flag. 
Now, instead of just failing, the error explains that you're hitting this because you explicitly used `--system`.

This closes #15639.

# Test plan

- I added a new integration test (`install_with_system_interpreter` in `pip_install.rs`) that checks the error message includes the hint.
- I tried to run `uv pip install --system -r requirements.txt` to see the actual error message in action, but had permission issues.

---

_@cmnemoi reviewed on 2025-10-15 16:38_

---

_Review comment by @cmnemoi on `crates/uv/tests/it/pip_install.rs`:13010 on 2025-10-15 16:38_

I am not sure if this is the best way to do this, or if this should be added to  `context.filters` directly.

---

_Review comment by @konstin on `crates/uv/src/commands/pip/install.rs`:247 on 2025-10-20 18:34_

We should show the hint in both branches

---

_Review comment by @konstin on `crates/uv/src/commands/pip/install.rs`:245 on 2025-10-20 18:35_

What about the following hint:

> The `--system` flag was used

We usually use indirect language instead of "you used"

---

_Review comment by @konstin on `crates/uv/src/commands/pip/install.rs`:245 on 2025-10-20 18:35_

We style the hint with:

```
"hint".bold().cyan(),
":".bold(),
```

---

_@konstin reviewed on 2025-10-20 18:35_

---

_@konstin approved on 2025-10-21 07:35_

Thank you!

---

_Label `error messages` added by @konstin on 2025-10-21 07:35_

---

_Merged by @konstin on 2025-10-21 07:46_

---

_Closed by @konstin on 2025-10-21 07:46_

---
