---
number: 17036
title: Fix relocatable nushell activation script
type: pull_request
state: closed
author: yumeminami
labels: []
assignees: []
base: main
head: fix/relocatable-venv-activation-nushell-scripts
created_at: 2025-12-08T17:47:17Z
updated_at: 2025-12-11T03:42:40Z
url: https://github.com/astral-sh/uv/pull/17036
synced_at: 2026-01-07T13:12:19-06:00
---

# Fix relocatable nushell activation script

---

_Pull request opened by @yumeminami on 2025-12-08 17:47_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Nushell activation now computes the venv root dynamically via path self for relocatable venvs, while non-relocatable venvs still embed a quoted absolute path

Still keep `activate.csh` (maybe delete is also an option)

close https://github.com/astral-sh/uv/issues/16973

<!-- How was it tested? -->


---

_@zanieb approved on 2025-12-08 17:52_

---

_Comment by @yumeminami on 2025-12-08 19:57_

ci failed due to timeout and how to rerun it.

---

_@cthoyt reviewed on 2025-12-09 09:55_

---

_Review comment by @cthoyt on `crates/uv-virtualenv/src/activator/activate.nu`:71 on 2025-12-09 09:55_

wouldn't the quotes be important in case there are spaces in the file path?

---

_Merged by @zanieb on 2025-12-09 14:39_

---

_Closed by @zanieb on 2025-12-09 14:39_

---

_Referenced in [Homebrew/homebrew-core#257965](../../Homebrew/homebrew-core/pulls/257965.md) on 2025-12-09 23:22_

---

_Branch deleted on 2025-12-11 03:42_

---
