```yaml
number: 2793
title: "Update `PASSWORD_NAMES` to include `api_key` and remove `pass`"
type: pull_request
state: closed
author: KittyBorgX
labels: []
assignees: []
base: main
head: main
created_at: 2023-02-12T02:56:10Z
updated_at: 2023-02-26T22:10:02Z
url: https://github.com/astral-sh/ruff/pull/2793
synced_at: 2026-01-12T04:39:44Z
```

# Update `PASSWORD_NAMES` to include `api_key` and remove `pass`

---

_Pull request opened by @KittyBorgX on 2023-02-12 02:56_

This updates `PASSWORD_NAMES` in `crates/ruff/src/rules/flake8_bandit/helpers.rs` as per #2384 

---

_@charliermarsh reviewed on 2023-02-12 18:16_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bandit/helpers.rs`:5 on 2023-02-12 18:16_

I'm wondering if, instead, we should consider using a regex to match Bandit's behavior?

https://github.com/PyCQA/bandit/blob/a9eaafa53ae73c1a66172373128cb19bb7b82a32/bandit/plugins/general_hardcoded_password.py#L13-L16

---

_@ngnpope reviewed on 2023-02-13 11:22_

---

_Review comment by @ngnpope on `crates/ruff/src/rules/flake8_bandit/helpers.rs`:5 on 2023-02-13 11:22_

Was going to say the same thing. This is a small improvement but doesn't solve the main problem I raised in the issue - that the current substring match causes matching parts of larger words.

---

_@KittyBorgX reviewed on 2023-02-13 14:11_

---

_Review comment by @KittyBorgX on `crates/ruff/src/rules/flake8_bandit/helpers.rs`:5 on 2023-02-13 14:11_

@charliermarsh would regex be in the scope of the linked issue? if so, i'll implement it in this pr.


---

_@charliermarsh reviewed on 2023-02-13 14:55_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bandit/helpers.rs`:5 on 2023-02-13 14:55_

Yeah I believe it's within scope.

---

_Comment by @charliermarsh on 2023-02-26 22:10_

Closing as this was resolved by #3222.

---

_Closed by @charliermarsh on 2023-02-26 22:10_

---
