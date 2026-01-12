```yaml
number: 5503
title: "Make `CachedEnvironment` robust to concurrent modifications"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-27T01:28:14Z
updated_at: 2024-07-29T00:32:12Z
url: https://github.com/astral-sh/uv/issues/5503
synced_at: 2026-01-12T15:58:56Z
```

# Make `CachedEnvironment` robust to concurrent modifications

---

_@charliermarsh_

Right now, it's possible that we'd modify an environment that's in-use by another process.

---

_Label `bug` added by @charliermarsh on 2024-07-27 01:28_

---

_Label `preview` added by @charliermarsh on 2024-07-27 01:28_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-27 01:28_

---

_Comment by @charliermarsh on 2024-07-27 02:08_

I think I know what to do here but it's slightly tricky. We might be able to reuse it for tool installs though. We want to create the environment in one location, then relocate it to the final location (probably by symlinking so that it's atomic). But we can't do this naively because we need the interpreter in any installed scripts to use the path of the final location.

---

_Closed by @charliermarsh on 2024-07-29 00:32_

---
