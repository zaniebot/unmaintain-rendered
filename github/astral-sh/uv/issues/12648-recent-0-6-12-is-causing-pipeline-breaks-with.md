---
number: 12648
title: "Recent 0.6.12 is causing pipeline breaks with error  invalid command 'bdist_wheel'"
type: issue
state: closed
author: fervand1
labels:
  - bug
assignees: []
created_at: 2025-04-03T10:34:04Z
updated_at: 2025-04-09T06:55:21Z
url: https://github.com/astral-sh/uv/issues/12648
synced_at: 2026-01-07T13:12:18-06:00
---

# Recent 0.6.12 is causing pipeline breaks with error  invalid command 'bdist_wheel'

---

_Issue opened by @fervand1 on 2025-04-03 10:34_

### Summary

The latest release 0.6.12 is breaking the pipelines which were previously working. The following error is raised.

```
      [stderr]
      error: invalid command 'bdist_wheel'

      hint: This error likely indicates that `mypackage @ file:///D:/test/myrepo/mypackage`
      depends on `wheel`, but doesn't declare it as a build dependency. If `mypackage @
      file:///D:/test/myrepo/mypackage` is a first-party package, consider adding `wheel` to
      its `build-system.requires`. Otherwise, `uv pip install wheel` into the environment and re-run with
      `--no-build-isolation`.
```
Although wheel is not the direct dependency, I have following configuration, where `wheel` is dependency of `mybuildtools` package.

```
[build-system]
requires = ["setuptools", "mybuildtools"]
build-backend = "setuptools.build_meta"
```

What has changed in the new release that is causing the issue and why it needs to be explicit dependency and not sub dependency? I think the previous version was doing it correctly.

Note: If I downgrade to 0.6.11, the pipelines are succeeding correctly

### Platform

Windows 11

### Version

0.6.12

### Python version

3.10

---

_Label `bug` added by @fervand1 on 2025-04-03 10:34_

---

_Comment by @fervand1 on 2025-04-03 12:39_

Closing this ticket, as the issue is more related to wheel v0.46.0 then uv itself

---

_Closed by @fervand1 on 2025-04-03 12:39_

---

_Comment by @fervand1 on 2025-04-03 13:20_

yanking wheel v0.46.0 resolved it

---

_Comment by @njzjz on 2025-04-09 06:55_

xref: https://github.com/pypa/wheel/issues/660


---
