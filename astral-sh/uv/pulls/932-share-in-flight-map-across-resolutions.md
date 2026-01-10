```yaml
number: 932
title: Share in-flight map across resolutions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pack
created_at: 2024-01-15T17:54:27Z
updated_at: 2024-01-15T18:11:23Z
url: https://github.com/astral-sh/uv/pull/932
synced_at: 2026-01-10T15:39:02Z
```

# Share in-flight map across resolutions

---

_Pull request opened by @charliermarsh on 2024-01-15 17:54_

## Summary

This PR fixes a subtle bug in `pip install` when using `--reinstall`. If a package depends on a build system directly (e.g., `waitress` depends on `setuptools`), and then you have other packages that also need the build system to build a source distribution, right now, we don't share the `OnceMap` between those cases.

This lifts the `InFlight` tracking up a level, so that it's initialized once per command, then shared everywhere.

## Test Plan

I'm having trouble coming up with an identical test-case and hesitant to add this slow test to the suite... But if you run `pip install --reinstall` with:

```
waitress @ git+https://github.com/zanieb/waitress
devpi-server @ git+https://github.com/zanieb/devpi#subdirectory=server
```

It fails consistently on `main` and passes here.

---

_Review requested from @konstin by @charliermarsh on 2024-01-15 17:54_

---

_Label `bug` added by @charliermarsh on 2024-01-15 17:54_

---

_Merged by @charliermarsh on 2024-01-15 18:11_

---

_Closed by @charliermarsh on 2024-01-15 18:11_

---

_Branch deleted on 2024-01-15 18:11_

---
