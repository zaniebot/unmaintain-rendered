```yaml
number: 15434
title: "fix(tests): Ensure show_settings tests don't load user config"
type: pull_request
state: merged
author: pposca
labels:
  - internal
assignees: []
merged: true
base: main
head: fix/15403-show-settings-tests
created_at: 2025-08-21T21:10:39Z
updated_at: 2025-08-22T19:05:00Z
url: https://github.com/astral-sh/uv/pull/15434
synced_at: 2026-01-12T16:11:44Z
```

# fix(tests): Ensure show_settings tests don't load user config

---

_@pposca_

## Summary

Ensure show_settings tests set XDG_CONFIG_HOME to avoid loading user configuration

Closes #15403



---

_Comment by @zanieb on 2025-08-21 21:35_

Thank you!

Did you test this?

I think we'll want to do a larger refactor such that this uses the shared environment variables from the test context and removes what it needs to, but we can track that separately.

---

_Label `internal` added by @zanieb on 2025-08-21 21:35_

---

_Comment by @pposca on 2025-08-21 21:45_

Yes, it is tested.

I use some uv user level config that made these tests fail. That's how I came across the issue, but with this fix all tests are passing.

Really happy to contribute.

---

_Merged by @zanieb on 2025-08-21 22:10_

---

_Closed by @zanieb on 2025-08-21 22:10_

---

_Branch deleted on 2025-08-22 19:05_

---
