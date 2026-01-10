```yaml
number: 5235
title: Avoid URL parsing when deserializing wheels
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/workspace
created_at: 2024-07-20T01:38:53Z
updated_at: 2024-07-20T02:11:27Z
url: https://github.com/astral-sh/uv/pull/5235
synced_at: 2026-01-10T13:42:52Z
```

# Avoid URL parsing when deserializing wheels

---

_Pull request opened by @charliermarsh on 2024-07-20 01:38_

## Summary

This PR slots in `UrlString` for `WheelWire`, which IIUC should avoid parsing URLs during deserialization?


---

_Review requested from @ibraheemdev by @charliermarsh on 2024-07-20 01:38_

---

_Label `performance` added by @charliermarsh on 2024-07-20 01:38_

---

_Marked ready for review by @charliermarsh on 2024-07-20 01:39_

---

_Comment by @charliermarsh on 2024-07-20 01:40_

If we want, I see a path to skipping URL parsing entirely except for those distributions that we attempt to install / query.

---

_@ibraheemdev approved on 2024-07-20 01:45_

---

_Comment by @ibraheemdev on 2024-07-20 01:46_

> If we want, I see a path to skipping URL parsing entirely except for those distributions that we attempt to install / query.

This would just mean using `UrlString` in more places, right?

---

_Comment by @charliermarsh on 2024-07-20 01:55_

> This would just mean using UrlString in more places, right?

I think we would change `File::try_from` to _not_ parse at all and instead store the raw string (this could be `UrlString`, but we'd need to change `UrlString` so that it's _not_ guaranteed to be a valid URL). Make `UrlString::to_url` a `Result` and add appropriate error handling.

---

_Comment by @charliermarsh on 2024-07-20 02:03_

I can give it a try at some point, but let me know if you beat me to it.

---

_Merged by @charliermarsh on 2024-07-20 02:11_

---

_Closed by @charliermarsh on 2024-07-20 02:11_

---

_Branch deleted on 2024-07-20 02:11_

---
