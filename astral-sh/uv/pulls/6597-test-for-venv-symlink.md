```yaml
number: 6597
title: Test for .venv symlink
type: pull_request
state: merged
author: hauntsaninja
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: symlink-test
created_at: 2024-08-25T08:19:12Z
updated_at: 2024-08-26T16:48:56Z
url: https://github.com/astral-sh/uv/pull/6597
synced_at: 2026-01-12T16:07:26Z
```

# Test for .venv symlink

---

_@hauntsaninja_

For various reasons, I have a preference for out of tree virtual environments. Things just work if I symlink, but I don't know that this is guaranteed, so I thought I'd add a test for it. It looks like there's another code path that matters (`FoundInterpreter::discover -> PythonEnvironment::from_root`) for the higher level commands, but couldn't spot a good place to test that.

Related discussion: https://github.com/astral-sh/uv/issues/1495#issuecomment-1950442191 / https://github.com/astral-sh/uv/issues/1578#issuecomment-1949911871

---

_@zanieb approved on 2024-08-26 16:44_

Works for me. If you wanted to add more coverage, the `python_find` tests are probably the place.

---

_Label `internal` added by @zanieb on 2024-08-26 16:44_

---

_Label `testing` added by @zanieb on 2024-08-26 16:44_

---

_Merged by @zanieb on 2024-08-26 16:44_

---

_Closed by @zanieb on 2024-08-26 16:44_

---

_Branch deleted on 2024-08-26 16:48_

---
