```yaml
number: 2215
title: HTML-decode URLs in HTML indexes
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - registry
assignees: []
merged: true
base: main
head: charlie/decode
created_at: 2024-03-05T19:21:38Z
updated_at: 2024-03-05T19:26:55Z
url: https://github.com/astral-sh/uv/pull/2215
synced_at: 2026-01-12T16:04:55Z
```

# HTML-decode URLs in HTML indexes

---

_@charliermarsh_

## Summary

If the index lists a URL like `https://buf.build/gen/python/hashb-foxglove-protocolbuffers-python/hashb_foxglove_protocolbuffers_python-25.3.0.1.20240226043130&#43;465630478360-py3-none-any.whl`, we need to decode that to `https://buf.build/gen/python/hashb-foxglove-protocolbuffers-python/hashb_foxglove_protocolbuffers_python-25.3.0.1.20240226043130+465630478360-py3-none-any.whl`.

Closes https://github.com/astral-sh/uv/issues/2202.


---

_Label `bug` added by @charliermarsh on 2024-03-05 19:21_

---

_Marked ready for review by @charliermarsh on 2024-03-05 19:21_

---

_Label `registry` added by @charliermarsh on 2024-03-05 19:21_

---

_Merged by @charliermarsh on 2024-03-05 19:26_

---

_Closed by @charliermarsh on 2024-03-05 19:26_

---

_Branch deleted on 2024-03-05 19:26_

---
