```yaml
number: 13501
title: Fix VIRTUAL_ENV_PROMPT value in activator/activate
type: pull_request
state: merged
author: ericbn
labels:
  - compatibility
assignees: []
merged: true
base: main
head: fix_virtual_env_prompt_in_activate
created_at: 2025-05-17T02:07:50Z
updated_at: 2025-05-17T16:08:34Z
url: https://github.com/astral-sh/uv/pull/13501
synced_at: 2026-01-10T11:10:41Z
```

# Fix VIRTUAL_ENV_PROMPT value in activator/activate

---

_Pull request opened by @ericbn on 2025-05-17 02:07_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
I've compared all the activator scripts here with the original ones in https://github.com/pypa/virtualenv/tree/main/src/virtualenv/activation and only the bash/POSIX script here was yielding a VIRTUAL_ENV_PROMPT value with parenthesis and a trailing space, which should be part of the shell prompt (PS1 for bash/POSIX) but not of the VIRTUAL_ENV_PROMPT value itself. This fixes that small inconsistency. Fixes #13456

This reverts commit 0ec2d4e434878c6f99f7fea77b5236292a35093a

## Test Plan

<!-- How was it tested? -->
I didn't test this locally.


---

_Comment by @charliermarsh on 2025-05-17 11:44_

I tend to agree with this change.

---

_Label `compatibility` added by @charliermarsh on 2025-05-17 11:48_

---

_Merged by @charliermarsh on 2025-05-17 11:48_

---

_Closed by @charliermarsh on 2025-05-17 11:48_

---

_Branch deleted on 2025-05-17 16:08_

---
