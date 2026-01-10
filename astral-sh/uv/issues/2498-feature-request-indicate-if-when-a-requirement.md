---
number: 2498
title: "Feature request: indicate if/when a requirement came from an extra"
type: issue
state: closed
author: wimglenn
labels:
  - duplicate
assignees: []
created_at: 2024-03-17T19:26:29Z
updated_at: 2024-03-17T19:29:08Z
url: https://github.com/astral-sh/uv/issues/2498
synced_at: 2026-01-10T01:23:18Z
---

# Feature request: indicate if/when a requirement came from an extra

---

_Issue opened by @wimglenn on 2024-03-17 19:26_

For example:

```
echo 'structlog[docs,tests]' | ./uv pip compile -
```

I would like the output to indicate if/when a requirement comes from an extra, e.g.

```
furo==2024.1.29
    # via structlog[docs]
...
simplejson==3.19.2
    # via structlog[tests]
```

Additionally, the top-level req itself should probably mention the extras requested, in this case it would be `structlog[docs,tests]==24.1.0`


---

_Comment by @zanieb on 2024-03-17 19:29_

Hi this is a duplicate of https://github.com/astral-sh/uv/issues/1595

---

_Closed by @zanieb on 2024-03-17 19:29_

---

_Label `duplicate` added by @zanieb on 2024-03-17 19:29_

---
