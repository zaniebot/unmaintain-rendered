```yaml
number: 17456
title: Isolate test environment from user configuration
type: pull_request
state: open
author: gaborbernat
labels: []
assignees: []
base: main
head: stable-ci
created_at: 2026-01-14T00:42:38Z
updated_at: 2026-01-14T03:31:41Z
url: https://github.com/astral-sh/uv/pull/17456
synced_at: 2026-01-14T04:30:51Z
```

# Isolate test environment from user configuration

---

_@gaborbernat_

Isolates the test environment from user-specific configuration that can cause test failures:

- **Internal package mirrors**: Remove `UV_INDEX_URL`, `UV_EXTRA_INDEX_URL`, `UV_PYTHON_INSTALL_MIRROR`, and `UV_INSTALLER_GHE_BASE_URL` from test environment to ensure tests use public PyPI instead of internal mirrors that may lack upload date metadata

- **User config files**: Set `XDG_CONFIG_HOME` in `with_real_home()` to prevent tests from reading user configuration files (like `.python-version`) that could override test-specified Python versions

- **Documentation**: Add guidance to CONTRIBUTING.md for developers behind corporate firewalls that block access to external test services like `pypi-proxy.fly.dev`


These were the one that I noticed and were a blocker for me to work on https://github.com/astral-sh/uv/pull/14728

---

_Comment by @zanieb on 2026-01-14 01:33_

Overlaps with #14080 

---
