```yaml
number: 15485
title: Add no clear option for uv venv
type: pull_request
state: closed
author: Aditya-PS-05
labels: []
assignees: []
base: main
head: add-no-clear-option
created_at: 2025-08-24T17:06:11Z
updated_at: 2025-09-11T22:52:38Z
url: https://github.com/astral-sh/uv/pull/15485
synced_at: 2026-01-12T16:11:46Z
```

# Add no clear option for uv venv

---

_@Aditya-PS-05_

closes #15475 

## Test Plan
I created tests for 4 scenarios
- no clear for existing directory
- no clear for blank directory
- no clear with clear
- no clear with allow existing


---

_@zanieb reviewed on 2025-08-24 17:19_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2697 on 2025-08-24 17:19_

I think we should use `overrides_with` for `clear` instead.

---

_@zanieb reviewed on 2025-08-24 17:20_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:653 on 2025-08-24 17:20_

I might change the `Fail` variant to have a `bool` for `allow_prompt` instead?

---

_Review requested from @zanieb by @Aditya-PS-05 on 2025-08-24 19:45_

---

_@zanieb reviewed on 2025-08-25 12:17_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:123 on 2025-08-25 12:17_

I'd use `match allow_prompt.then(|| confirm_clear(..)).flatten()` so we don't need to indent the match and repeat the error message construction.

---

_@zanieb reviewed on 2025-08-25 12:20_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:123 on 2025-08-25 12:20_

(You might need `.transpose()?.flatten()` to handle the error still)

---

_@harshithvh reviewed on 2025-09-11 18:58_

---

_Review comment by @harshithvh on `crates/uv-virtualenv/src/virtualenv.rs`:123 on 2025-09-11 18:58_

resolved at #15795 

---

_Closed by @Aditya-PS-05 on 2025-09-11 22:52_

---
