```yaml
number: 3222
title: "[bandit]: Do not treat \"passed\" as \"password\" for `S105`/`S106`/`S107`"
type: pull_request
state: merged
author: edgarrmondragon
labels: []
assignees: []
merged: true
base: main
head: fix/bandit-password-regex
created_at: 2023-02-25T03:31:05Z
updated_at: 2023-02-25T21:36:54Z
url: https://github.com/astral-sh/ruff/pull/3222
synced_at: 2026-01-12T04:39:44Z
```

# [bandit]: Do not treat "passed" as "password" for `S105`/`S106`/`S107`

---

_Pull request opened by @edgarrmondragon on 2023-02-25 03:31_

Closes #2384

cc @KittyBorgX

---

_@not-my-profile reviewed on 2023-02-25 11:36_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/flake8_bandit/helpers.rs`:9 on 2023-02-25 11:36_

There is absolutely no need to repeat `(pas+wo?r?d|pass(phrase)?|pwd|token|secrete?)` four times within this regex.

---

_@not-my-profile reviewed on 2023-02-25 11:37_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/flake8_bandit/helpers.rs`:9 on 2023-02-25 11:37_

The regex could be simplified to `(^|_)(pas+wo?r?d|pass(phrase)?|pwd|token|secrete?)($|_)`.

---

_@charliermarsh reviewed on 2023-02-25 16:40_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bandit/helpers.rs`:9 on 2023-02-25 16:40_

I don't fully understand why they wrote it out in that way, but what @edgarrmondragon included here looks like a direct port of the version in Bandit: https://github.com/PyCQA/bandit/blob/a9eaafa53ae73c1a66172373128cb19bb7b82a32/bandit/plugins/general_hardcoded_password.py#L13-L16

---

_Review comment by @edgarrmondragon on `crates/ruff/src/rules/flake8_bandit/helpers.rs`:9 on 2023-02-25 18:18_

You're right @charliermarsh, I just ported the Python regex.

@not-my-profile thanks for the suggestion, it makes total sense


---

_@edgarrmondragon reviewed on 2023-02-25 18:18_

---

_Merged by @charliermarsh on 2023-02-25 20:32_

---

_Closed by @charliermarsh on 2023-02-25 20:32_

---

_Branch deleted on 2023-02-25 21:36_

---
