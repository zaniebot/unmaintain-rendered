```yaml
number: 10218
title: Does uv recognize url without https schema?
type: issue
state: open
author: youkaichao
labels:
  - needs-decision
  - cli
assignees: []
created_at: 2024-12-29T01:36:23Z
updated_at: 2025-01-06T20:34:54Z
url: https://github.com/astral-sh/uv/issues/10218
synced_at: 2026-01-12T16:00:09Z
```

# Does uv recognize url without https schema?

---

_@youkaichao_

I'm trying to get vllm nightly wheels working for `uv`, and would like to simplify the user interface.

Right now, I get:

this is working:

`uv pip install vllm --extra-index-url https://wheels.vllm.ai/nightly/`

this is not working:

`uv pip install vllm --extra-index-url wheels.vllm.ai/nightly/`

is it necesarry for users to specify the `https` schema? or is it possible for uv to add `https` by default?

I'm using uv 0.5.11

cc @charliermarsh 

---

_Comment by @charliermarsh on 2024-12-29 01:43_

We might be able to assume HTTPS, although pip requires the schema so I'm a little hesitant to diverge. (Users can also provide a file path there, but we require a `file://` schema, so it wouldn't be ambiguous.)

---

_Comment by @youkaichao on 2024-12-29 01:48_

as long as we have a clear documentation about the diverging behavior, I think it is fine, especially for some user-friendly behaviors we are asking for a long time (and `pip` will not add).

`pip` is too slow to evolve :(

---

_Label `needs-decision` added by @zanieb on 2025-01-06 20:34_

---

_Label `cli` added by @zanieb on 2025-01-06 20:34_

---
