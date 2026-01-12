```yaml
number: 15990
title: "Retain the cache lock and temporary caches during `uv run` and `uvx`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/no-drop-cache
created_at: 2025-09-22T20:04:00Z
updated_at: 2025-09-22T20:41:09Z
url: https://github.com/astral-sh/uv/pull/15990
synced_at: 2026-01-12T16:12:03Z
```

# Retain the cache lock and temporary caches during `uv run` and `uvx`

---

_@zanieb_

We're seeing reports of a regression from https://github.com/astral-sh/uv/pull/15888 where `--no-cache` causes `uv run` and `uvx` to fail to spawn a command.

The intent of this code was to allow destructive cache operations _after_ we'd finished setting up the environment. However, it's unclear to me that it's safe to run `uv cache clean` during a `uv run` operation (e.g., `uv run --script` uses an environment in the cache) and, more importantly, we cannot drop non-persistent caches (e.g., from `--no-cache`) as they include the environment we're spawning the command in.

Alternative to #15977 which retains release of the lock — we may want to consider that approach still but this regression needs to be resolved quickly. 

Closes https://github.com/astral-sh/uv/issues/15989
Closes https://github.com/astral-sh/uv/issues/15987
Closes https://github.com/astral-sh/uv/issues/15967

---

_Label `bug` added by @zanieb on 2025-09-22 20:04_

---

_@charliermarsh approved on 2025-09-22 20:12_

---

_@geofft approved on 2025-09-22 20:12_

---

_@woodruffw approved on 2025-09-22 20:14_

---

_Comment by @zanieb on 2025-09-22 20:16_

The downside to this approach is that if uv run runs forever, e.g., a server, uv cache clean will block forever too. I'm not sure what to do about that, other than some derivative of the implementation in https://github.com/astral-sh/uv/pull/15977 which accounts for cases like `uv run --script`. Or, we could add a way to bypass the lock, e.g., `uv cache clean --force`.

---

_Merged by @zanieb on 2025-09-22 20:41_

---

_Closed by @zanieb on 2025-09-22 20:41_

---

_Branch deleted on 2025-09-22 20:41_

---
