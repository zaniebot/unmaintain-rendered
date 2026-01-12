```yaml
number: 2794
title: "[feature request] python bindings for uv api"
type: issue
state: closed
author: gmankab
labels:
  - wontfix
assignees: []
created_at: 2024-04-03T03:00:51Z
updated_at: 2024-04-03T04:01:41Z
url: https://github.com/astral-sh/uv/issues/2794
synced_at: 2026-01-12T15:58:40Z
```

# [feature request] python bindings for uv api

---

_@gmankab_

in python you can do this:

```python
import venv
venv.create()
```

can you please provide python bindings for uv api, so then you can do this:

```python
import uv.venv
uv.venv.create()
```


---

_Comment by @zanieb on 2024-04-03 04:01_

Hi! Thanks for your request, but we won't be supporting this anytime soon. The more APIs we need to maintain the less we can focus on innovation. Right now, we're focusing on our CLI as the canonical way to interact with `uv`. For some use-cases, we are willing to make changes to our Rust API but that is very unstable.

If you're reading this issue feel free to 👍 the original post if you're interested in this.

We're likely to have a programmatic API someday, but not now.

---

_Closed by @zanieb on 2024-04-03 04:01_

---

_Label `wontfix` added by @zanieb on 2024-04-03 04:01_

---
