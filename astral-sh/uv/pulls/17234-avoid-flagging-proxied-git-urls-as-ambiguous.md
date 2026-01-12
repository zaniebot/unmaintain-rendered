```yaml
number: 17234
title: Avoid flagging proxied Git URLs as ambiguous authority
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ambiguous-authority
created_at: 2025-12-24T14:07:12Z
updated_at: 2025-12-25T11:42:22Z
url: https://github.com/astral-sh/uv/pull/17234
synced_at: 2026-01-12T16:12:40Z
```

# Avoid flagging proxied Git URLs as ambiguous authority

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/17214.


---

_Review requested from @woodruffw by @charliermarsh on 2025-12-24 14:07_

---

_@woodruffw approved on 2025-12-25 04:19_

LGTM!

I was surprised by this, but these kinds of nested URLs are valid according to both RFC 3986 and WHATWG (the nuance for WHATWG being that `git+https` isn't a recognized browser scheme, but that doesn't matter in our context).

---

_Marked ready for review by @charliermarsh on 2025-12-25 11:42_

---

_Merged by @charliermarsh on 2025-12-25 11:42_

---

_Closed by @charliermarsh on 2025-12-25 11:42_

---

_Branch deleted on 2025-12-25 11:42_

---
