```yaml
number: 18491
title: Fix doc for Neovim setting examples
type: pull_request
state: merged
author: shimies
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-docs-for-nvim-settings
created_at: 2025-06-06T09:35:16Z
updated_at: 2025-06-06T09:50:57Z
url: https://github.com/astral-sh/ruff/pull/18491
synced_at: 2026-01-10T18:45:04Z
```

# Fix doc for Neovim setting examples

---

_Pull request opened by @shimies on 2025-06-06 09:35_

## Summary
This PR fixes an error in the example Neovim configuration on [this documentation page](https://docs.astral.sh/ruff/editors/settings/#configuration).
The `configuration` block should be nested under `settings`, consistent with other properties and as outlined [here](https://docs.astral.sh/ruff/editors/setup/#neovim).

I encountered this issue when copying the example to configure ruff integration in my neovim - the config didn’t work until I corrected the nesting.

## Test Plan
- [x] Confirmed that the corrected configuration works in a real Neovim + Ruff setup
- [x] Verified that the updated configuration renders correctly in MkDocs
<img width="382" alt="image" src="https://github.com/user-attachments/assets/0722fb35-8ffa-4b10-90ba-c6e8417e40bf" />



---

_Label `documentation` added by @dhruvmanila on 2025-06-06 09:46_

---

_@dhruvmanila approved on 2025-06-06 09:47_

Thank you!

---

_Renamed from "Fix doc for nvim setting examples" to "Fix doc for Neovim setting examples" by @dhruvmanila on 2025-06-06 09:48_

---

_Merged by @dhruvmanila on 2025-06-06 09:49_

---

_Closed by @dhruvmanila on 2025-06-06 09:49_

---

_Branch deleted on 2025-06-06 09:50_

---
