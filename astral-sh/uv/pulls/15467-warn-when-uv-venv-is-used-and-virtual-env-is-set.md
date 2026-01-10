```yaml
number: 15467
title: Warn when uv venv is used and VIRTUAL_ENV is set
type: pull_request
state: open
author: harsh-ps-2003
labels: []
assignees: []
base: main
head: cli
created_at: 2025-08-23T09:38:01Z
updated_at: 2025-08-29T17:58:58Z
url: https://github.com/astral-sh/uv/pull/15467
synced_at: 2026-01-10T06:44:33Z
```

# Warn when uv venv is used and VIRTUAL_ENV is set

---

_Pull request opened by @harsh-ps-2003 on 2025-08-23 09:38_

<!--
Thank you for contributing to UV! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

@zanieb outlined in issue #14793 that when users set their own `VIRTUAL_ENV=/tmp/broken`, the `uv pip install` command fails with a broken `VIRTUAL_ENV` environment, instead of using the newly created `.venv`. A warning should have been there to inform the user. 

I have added the warning, only when the default path is exactly `.venv`.  This prevents the warning from appearing when:
* User runs `uv venv .env` (explicit path)
* User runs `uv venv venvs/foo` (custom name)
* Project environment uses a different name than `.venv`

The warning added :
```
warning: `VIRTUAL_ENV=[TEMP_DIR]/other` is set; subsequent commands may use the active environment instead of `.venv`. Consider clearing it with `deactivate`.
```

## Test Plan

A test `create_venv_warns_when_virtual_env_is_set_and_default_used()` has been added apart from manual testing :

```
harshps22ugp@lab:~/projects/uv$ VIRTUAL_ENV=/tmp/broken ./target/release/uv venv
Using CPython 3.14.0rc2
Creating virtual environment at: .venv
warning: `VIRTUAL_ENV=/tmp/broken` is set; subsequent commands may use the active environment instead of `.venv`. Consider clearing it with `deactivate`.
✔ A virtual environment already exists at `.venv`. Do you want to replace it? · yes
```


---

_Assigned to @zanieb by @zanieb on 2025-08-23 14:38_

---
