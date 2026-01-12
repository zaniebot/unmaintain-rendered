```yaml
number: 9992
title: Fix show_settings tests not to be affected by system configs
type: pull_request
state: merged
author: mgorny
labels:
  - testing
assignees: []
merged: true
base: main
head: test-settings
created_at: 2024-12-18T09:47:20Z
updated_at: 2024-12-18T16:02:15Z
url: https://github.com/astral-sh/uv/pull/9992
synced_at: 2026-01-12T16:09:04Z
```

# Fix show_settings tests not to be affected by system configs

---

_@mgorny_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Override XDG_CONFIG_DIRS in show_settings tests, in order to ensure that they don't pick system configuration, and therefore fail due to value mismatches.  This specifically addresses test failures on Gentoo where a default `/etc/xdg/uv/uv.toml` is installed, and users are free to modify it.

Prior to #9914, we used to set `XDG_CONFIG_DIRS` locally before running the test suite.  However, since the test now wipes the environment, the problem can no longer be resolved downstream.

## Test Plan

`cargo test` on a Gentoo system (with `/etc/xdg/uv/uv.toml` present).


---

_@charliermarsh reviewed on 2024-12-18 15:08_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/show_settings.rs`:14 on 2024-12-18 15:08_

It's a little strange because this directory will be dropped and deleted as soon as the method returns.

---

_Comment by @charliermarsh on 2024-12-18 15:10_

@mgorny -- I modified it to use the test context directory. Does that work?

---

_Label `testing` added by @charliermarsh on 2024-12-18 15:10_

---

_@zanieb approved on 2024-12-18 15:18_

---

_Review comment by @mgorny on `crates/uv/tests/it/show_settings.rs`:14 on 2024-12-18 15:26_

That's what I get for doing guesswork copy&paste. I guess it was happy with any value, whether it existed or not. I'll test your version in a minute, just lemme finish with triton first, since my home is not large enough for uv and something else :-).

---

_@mgorny reviewed on 2024-12-18 15:26_

---

_@charliermarsh reviewed on 2024-12-18 15:30_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/show_settings.rs`:14 on 2024-12-18 15:30_

Haha no worries. I think what you had probably worked...? Just a bit unusual since `XDG_CONFIG_DIRS` was pointing to a non-existent directory.

---

_@mgorny reviewed on 2024-12-18 15:57_

---

_Review comment by @mgorny on `crates/uv/tests/it/show_settings.rs`:14 on 2024-12-18 15:57_

Well, if I were to go lawyering:

> If $XDG_CONFIG_DIRS is either not set or empty, a value equal to /etc/xdg should be used.

It doesn't say anything about non-existing, so I guess any value is sufficient to disable `/etc/xdg`. Still, I guess it's safer to use a real directory here to avoid any future issues.

---

_Comment by @mgorny on 2024-12-18 15:57_

Confirmed that the updated version works. Thanks a lot!

---

_Merged by @charliermarsh on 2024-12-18 16:02_

---

_Closed by @charliermarsh on 2024-12-18 16:02_

---
