```yaml
number: 1824
title: Rework --clear flag for venv command
type: pull_request
state: closed
author: flyaroundme
labels: []
assignees: []
draft: true
base: main
head: venv-clear-flag
created_at: 2024-02-21T18:47:55Z
updated_at: 2024-05-02T00:55:36Z
url: https://github.com/astral-sh/uv/pull/1824
synced_at: 2026-01-12T16:04:45Z
```

# Rework --clear flag for venv command

---

_@flyaroundme_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Reworked the --clear flag for venv command for it not to recreate the existing virtualenv unless --clear flag is set.

Based on this discussion https://github.com/astral-sh/uv/issues/1472

## Test Plan

Create venv using `uv venv` command. Then use `uv venv` command again without flag. Check that virtualenv is not cleared. Then use `uv venv --clear`, check that it is recreated.

```bash
$ uv venv
Using Python 3.10.12 interpreter at /usr/bin/python3.10
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
$ uv pip install ruff # fill virtualenv with something
Resolved 1 package in 141ms
Installed 1 package in 21ms
 + ruff==0.2.2
$ uv pip freeze # memorize the previous state of virtualenv
ruff==0.2.2
$ uv venv # without flag
Using Python 3.10.12 interpreter at /usr/bin/python3.10
Virtualenv already exists at .venv. Use --clear option to recreate
$ uv pip freeze # check that virtualenv is not cleared
ruff==0.2.2
$ uv venv --clear # with flag
Using Python 3.10.12 interpreter at /usr/bin/python3.10
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
$ uv pip freeze # check virtualenv is cleared
$
```


---

_Converted to draft by @flyaroundme on 2024-02-21 18:50_

---

_@T-256 reviewed on 2024-02-22 12:53_

---

_Review comment by @T-256 on `crates/uv/src/commands/venv.rs`:126 on 2024-02-22 12:53_

I'd rather to fail with non-zero exit code.
Also, message could be more strict since the command failing:
```
Failed to create virtualenv at '{}': Already exists.
hint: Use --clear option to force recreate new virtualenv at {}
```


---

_Comment by @T-256 on 2024-02-25 17:57_

IMO, We could also consider `--force` instead of `--clear` to also fix #1863 where target path is exists but is not venv.

---

_Comment by @zanieb on 2024-04-22 21:16_

Quite a bit of discussion about this over in https://github.com/astral-sh/uv/pull/2548

---

_Closed by @flyaroundme on 2024-05-02 00:55_

---
