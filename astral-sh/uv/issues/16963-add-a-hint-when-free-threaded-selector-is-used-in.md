```yaml
number: 16963
title: "Add a hint when free-threaded selector is used in `requires-python`"
type: issue
state: open
author: zanieb
labels:
  - enhancement
  - help wanted
  - error messages
assignees: []
created_at: 2025-12-03T13:33:45Z
updated_at: 2025-12-08T19:24:26Z
url: https://github.com/astral-sh/uv/issues/16963
synced_at: 2026-01-10T03:11:35Z
```

# Add a hint when free-threaded selector is used in `requires-python`

---

_Issue opened by @zanieb on 2025-12-03 13:33_

### Summary

See https://github.com/astral-sh/uv/issues/16951

We should be able to suggest a resolution here

### Example

```
error: Failed to parse: `pyproject.toml`
  Caused by: TOML parse error at line 4, column 19
  |
4 | requires-python = ">=3.14t"
  |                   ^^^^^^^^^
Failed to parse version: after parsing `3.14`, found `t`, which is not part of a valid version:
>=3.14t
^^^^^^^

hint: `requires-python` cannot include a free-threaded selector, consider using `uv python pin 3.14t` instead
```

---

_Label `enhancement` added by @zanieb on 2025-12-03 13:33_

---

_Label `error messages` added by @zanieb on 2025-12-03 13:33_

---

_Label `help wanted` added by @zanieb on 2025-12-03 13:34_

---

_Comment by @kxzk on 2025-12-04 01:37_

@zanieb mind if I take a crack at this?

---

_Comment by @zanieb on 2025-12-04 03:01_

Go for it!

---

_Comment by @zanieb on 2025-12-04 03:02_

(It might not be very easy)

---

_Comment by @kxzk on 2025-12-04 15:25_

@zanieb Got it, will give it a shot!

---

_Comment by @kxzk on 2025-12-08 19:24_

@zanieb PR is up, when you get a chance - can't seem to request review in PR. Thanks!

---
