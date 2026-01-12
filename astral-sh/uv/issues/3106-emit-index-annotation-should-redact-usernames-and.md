```yaml
number: 3106
title: "`--emit-index-annotation` should redact usernames and passwords in index URL"
type: issue
state: closed
author: nicodemus26
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-04-17T20:22:12Z
updated_at: 2024-04-18T03:59:45Z
url: https://github.com/astral-sh/uv/issues/3106
synced_at: 2026-01-12T15:58:41Z
```

# `--emit-index-annotation` should redact usernames and passwords in index URL

---

_@nicodemus26_

e.g.

```
# from https://aws:asupersecretpasswordorauthtoken@my-account-123.d.codeartifact.us-west-2.amazonaws.com/pypi/my-private-repo/simple
```
->
```
# from https://my-account-123.d.codeartifact.us-west-2.amazonaws.com/pypi/my-private-repo/simple
```

---

_Label `bug` added by @charliermarsh on 2024-04-17 20:22_

---

_Comment by @charliermarsh on 2024-04-17 20:22_

Makes sense!

---

_Label `help wanted` added by @charliermarsh on 2024-04-17 20:22_

---

_Comment by @zanieb on 2024-04-17 20:39_

See also https://github.com/astral-sh/uv/issues/1714

---

_Comment by @zanieb on 2024-04-17 20:40_

Something like... https://github.com/astral-sh/uv/blob/c0efeeddf6d738991d8f3149168ce57c52073f4e/crates/uv-auth/src/middleware.rs#L95

---

_Renamed from "`--emit-index-annotation` should redact passwords in index URL" to "`--emit-index-annotation` should redact usernames and passwords in index URL" by @nicodemus26 on 2024-04-17 20:45_

---

_Closed by @charliermarsh on 2024-04-18 03:59_

---
