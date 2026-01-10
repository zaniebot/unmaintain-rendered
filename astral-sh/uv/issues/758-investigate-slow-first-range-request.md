---
number: 758
title: Investigate slow first range request
type: issue
state: closed
author: konstin
labels:
  - performance
assignees: []
created_at: 2024-01-03T21:53:13Z
updated_at: 2024-04-08T08:36:50Z
url: https://github.com/astral-sh/uv/issues/758
synced_at: 2026-01-10T01:23:04Z
---

# Investigate slow first range request

---

_Issue opened by @konstin on 2024-01-03 21:53_

The first request of fetching range metadata is notably slow, in the no cache case sometimes the single biggest item (https://github.com/astral-sh/puffin/pull/744#issuecomment-1875153595). We should investigate why that is and if we can optimize it.

---

_Label `performance` added by @konstin on 2024-01-03 21:53_

---

_Comment by @charliermarsh on 2024-04-07 00:19_

@konstin - Do you think this was just the thing where we had to add the extra `await`, to kick-start the threading?

---

_Comment by @konstin on 2024-04-08 08:36_

yeah this is not a problem anymore

---

_Closed by @konstin on 2024-04-08 08:36_

---
