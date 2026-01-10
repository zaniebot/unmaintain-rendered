```yaml
number: 10221
title: uv python install and uninstall now account for the UV_PYTHON env var
type: pull_request
state: closed
author: Choudhry18
labels: []
assignees: []
base: main
head: env_var_python_install
created_at: 2024-12-29T23:40:35Z
updated_at: 2024-12-29T23:49:59Z
url: https://github.com/astral-sh/uv/pull/10221
synced_at: 2026-01-10T11:44:38Z
```

# uv python install and uninstall now account for the UV_PYTHON env var

---

_Pull request opened by @Choudhry18 on 2024-12-29 23:40_

# Summary

Checks UV_PYTHON for version when `uv python install` or `uv python uninstall ` ran

## Test Plan

I add a new snapshot test uv/tests/it/python_install::python_install_with_uv_python_env that sets UV_PYTHON to 3.12 which is not the latest version and the expected behavior is to install python 3.12.8 not the latest version. 

## Requirement Concern

The change makes it so that the version specified through UV_PYTHON takes precedence over a target version like `uv python install 3.8` and the python-versions file. I am unsure if that is the required behavior. The documentation says "Equivalent to the --python command-line argument. If set to a path, uv will use this Python interpreter for all operations."


---

_Closed by @Choudhry18 on 2024-12-29 23:49_

---
