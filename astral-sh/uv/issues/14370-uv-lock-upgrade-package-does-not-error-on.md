```yaml
number: 14370
title: "`uv lock --upgrade-package` does not error on misspelled package name"
type: issue
state: closed
author: johnthagen
labels:
  - bug
assignees: []
created_at: 2025-06-30T11:31:20Z
updated_at: 2025-06-30T23:41:32Z
url: https://github.com/astral-sh/uv/issues/14370
synced_at: 2026-01-10T03:32:45Z
```

# `uv lock --upgrade-package` does not error on misspelled package name

---

_Issue opened by @johnthagen on 2025-06-30 11:31_

### Summary

1. Create a new `uv` project
2. Run the following command in it, where you have misspelled a package name or otherwise entered one that does not exist

```
uv lock --upgrade-package batman-lives-on
```

No error is printed, even though `batman-lives-on` does not exist in the current project. It is not obvious to the user why nothing happened. In this case I chose a wildly wrong name, but also consider

```
uv lock --upgrade-package fatsapi
```

I would expect an error to be presented that says:

```
batman-lives-on is not dependency of the current project
```

### Platform

macOS 14 arm64

### Version

uv 0.7.17 (41c218a89 2025-06-29)

### Python version

3.12.10

---

_Label `bug` added by @johnthagen on 2025-06-30 11:31_

---

_Comment by @konstin on 2025-06-30 13:48_

We should catch those cases and display a warning if the package is not part of the input lock or  output resolution.

---

_Comment by @charliermarsh on 2025-06-30 23:41_

I think we should merge this with https://github.com/astral-sh/uv/issues/7805 (it's the same idea, just a different top-level command).

---

_Closed by @charliermarsh on 2025-06-30 23:41_

---
