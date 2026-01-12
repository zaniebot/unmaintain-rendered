```yaml
number: 13012
title: "Fix `SourceNotAllowed` error message"
type: pull_request
state: merged
author: j178
labels:
  - error messages
assignees: []
merged: true
base: main
head: list-py-fix
created_at: 2025-04-21T03:15:09Z
updated_at: 2025-04-21T12:36:32Z
url: https://github.com/astral-sh/uv/pull/13012
synced_at: 2026-01-12T16:10:30Z
```

# Fix `SourceNotAllowed` error message

---

_@j178_

## Summary

Before:

```console
$ uv python list py --managed-python
error: Interpreter discovery for `executable name `py`` requires `search path` but only only managed is allowed
```

After:

```console
$ uv python list py --managed-python
error: Interpreter discovery for `executable name `py`` requires `search path` but only `only managed` is allowed
```


---

_Closed by @j178 on 2025-04-21 03:22_

---

_Reopened by @j178 on 2025-04-21 03:22_

---

_Merged by @charliermarsh on 2025-04-21 12:29_

---

_Closed by @charliermarsh on 2025-04-21 12:29_

---

_Label `error messages` added by @charliermarsh on 2025-04-21 12:29_

---

_Branch deleted on 2025-04-21 12:36_

---
