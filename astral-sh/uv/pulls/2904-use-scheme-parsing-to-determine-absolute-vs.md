```yaml
number: 2904
title: Use scheme parsing to determine absolute vs. relative URLs
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/scheme
created_at: 2024-04-08T17:27:57Z
updated_at: 2024-04-08T21:04:27Z
url: https://github.com/astral-sh/uv/pull/2904
synced_at: 2026-01-10T14:43:31Z
```

# Use scheme parsing to determine absolute vs. relative URLs

---

_Pull request opened by @charliermarsh on 2024-04-08 17:27_

## Summary

We have a heuristic in `File` that attempts to detect whether a URL is absolute or relative. However, `contains("://")` is prone to false positive. In the linked issues, the URLs look like:

```
/packages/5a/d8/4d75d1e4287ad9d051aab793c68f902c9c55c4397636b5ee540ebd15aedf/pytz-2005k.tar.bz2?hash=597b596dc1c2c130cd0a57a043459c3bd6477c640c07ac34ca3ce8eed7e6f30c&remote=https://files.pythonhosted.org/packages/5a/d8/4d75d1e4287ad9d051aab793c68f902c9c55c4397636b5ee540ebd15aedf/pytz-2005k.tar.bz2#sha256=597b596dc1c2c130cd0a57a043459c3bd6477c640c07ac34ca3ce8eed7e6f30c
```

Which is relative, but includes `://`.

Instead, we should determine whether the URL has a _scheme_ which matches the `Url` crate internally.

Closes https://github.com/astral-sh/uv/issues/2899.


---

_Review requested from @zanieb by @charliermarsh on 2024-04-08 17:28_

---

_Marked ready for review by @charliermarsh on 2024-04-08 17:28_

---

_Label `bug` added by @charliermarsh on 2024-04-08 17:28_

---

_@charliermarsh reviewed on 2024-04-08 17:29_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/base_url.rs`:11 on 2024-04-08 17:29_

I don't think the `try` behavior is necessary here. `url::ParseError::RelativeUrlWithoutBase` occurs if and only if the URL has no scheme... And we already verify that `FileLocation::RelativeUrl` is created iff the URL has no scheme.

---

_@zanieb approved on 2024-04-08 20:33_

---

_Merged by @charliermarsh on 2024-04-08 21:04_

---

_Closed by @charliermarsh on 2024-04-08 21:04_

---

_Branch deleted on 2024-04-08 21:04_

---
