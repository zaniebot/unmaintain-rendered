```yaml
number: 15013
title: Ignore GraalPy devtag
type: pull_request
state: merged
author: msimacek
labels:
  - bug
assignees: []
merged: true
base: main
head: msimacek/graalpy
created_at: 2025-08-01T14:05:50Z
updated_at: 2025-08-07T20:53:37Z
url: https://github.com/astral-sh/uv/pull/15013
synced_at: 2026-01-12T16:11:31Z
```

# Ignore GraalPy devtag

---

_@msimacek_

Allows [development builds of GraalPy](https://github.com/graalvm/graal-languages-ea-builds) to work with uv.

CC @timfel



---

_Comment by @zanieb on 2025-08-01 16:55_

Could you add some sort of test coverage for this?

---

_Comment by @msimacek on 2025-08-01 20:06_

I added an integration test for running with a graalpy dev build. Without my changes, it would fail with
```
error: Failed to inspect Python interpreter from provided path at `graalpy-25.0.0-dev-linux-amd64/bin/python`
  Caused by: Querying Python at `graalpy-25.0.0-dev-linux-amd64/bin/python` returned an invalid response: expected version to start with a number, but no leading ASCII digits were found
```
Is that ok?

---

_Label `bug` added by @zanieb on 2025-08-07 20:53_

---

_@zanieb approved on 2025-08-07 20:53_

Thanks!

---

_Merged by @zanieb on 2025-08-07 20:53_

---

_Closed by @zanieb on 2025-08-07 20:53_

---
