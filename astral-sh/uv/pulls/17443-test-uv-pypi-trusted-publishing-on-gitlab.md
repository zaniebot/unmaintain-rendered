```yaml
number: 17443
title: Test uv+PyPI Trusted Publishing on Gitlab
type: pull_request
state: open
author: woodruffw
labels: []
assignees: []
draft: true
base: main
head: ww/pypi-tp-gl-test
created_at: 2026-01-13T19:14:18Z
updated_at: 2026-01-13T19:23:31Z
url: https://github.com/astral-sh/uv/pull/17443
synced_at: 2026-01-13T19:37:41Z
```

# Test uv+PyPI Trusted Publishing on Gitlab

---

_@woodruffw_

## Summary

WIP.

The basic idea here is to invoke a Gitlab Pipeline via GitHub. That pipeline generates an OIDC token which it saves as an artifact, which the GitHub workflow can then read. We then use that OIDC token to impersonate the identity of the Gitlab Pipeline for Trusted Publishing purposes.

## Test Plan

Implement the above within `test_publish.py` and `ci.yml`.

---

_Assigned to @woodruffw by @woodruffw on 2026-01-13 19:14_

---
