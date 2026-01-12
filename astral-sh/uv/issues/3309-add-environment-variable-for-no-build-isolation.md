```yaml
number: 3309
title: "Add environment variable for `--no-build-isolation`"
type: issue
state: closed
author: morotti
labels:
  - configuration
assignees: []
created_at: 2024-04-29T14:25:54Z
updated_at: 2024-04-30T04:18:34Z
url: https://github.com/astral-sh/uv/issues/3309
synced_at: 2026-01-12T15:58:42Z
```

# Add environment variable for `--no-build-isolation`

---

_@morotti_

Hello,

I am trying to use the `--no-build-isolation` option, introduced in https://github.com/astral-sh/uv/pull/2258
It's necessary to make uv/pip use the current venv when installing or building packages. 
It's largely used in build systems and environments where a venv is already prepared to build/install packages.

I am finding that the option is not working when set by an environment variable? I think it might not be implemented.
Or the environment variable name/value might be something else. I've tried a few variants I could think of but that did not help.

```
# pass
uv pip install -e . --no-build-isolation
```

```
# fail
UV_NO_BUILD_ISOLATION=true uv pip install -e .
```

uv version 0.1.39 (latest as of today)



---

_Comment by @zanieb on 2024-04-29 14:37_

Yeah, we don't provide an environment variable for this yet.

---

_Label `configuration` added by @zanieb on 2024-04-29 14:37_

---

_Renamed from "UV_NO_BUILD_ISOLATION=true has no effect" to "Add environment variable for `--no-build-isolation`" by @zanieb on 2024-04-29 14:38_

---

_Closed by @charliermarsh on 2024-04-30 04:18_

---
