```yaml
number: 14403
title: "Includes `sys.prefix` in cached environment keys to avoid `--with` collisions across projects"
type: pull_request
state: merged
author: oconnor663
labels: []
assignees: []
merged: true
base: main
head: jack/env_cache_key
created_at: 2025-07-01T19:59:46Z
updated_at: 2025-07-02T19:40:20Z
url: https://github.com/astral-sh/uv/pull/14403
synced_at: 2026-01-10T06:53:01Z
```

# Includes `sys.prefix` in cached environment keys to avoid `--with` collisions across projects

---

_Pull request opened by @oconnor663 on 2025-07-01 19:59_

Fixes https://github.com/astral-sh/uv/issues/12889.

---

_Comment by @oconnor663 on 2025-07-02 16:45_

Now using both the sys_executable and the sys_prefix in the cache key.

---

_Review requested from @zanieb by @oconnor663 on 2025-07-02 16:46_

---

_Marked ready for review by @oconnor663 on 2025-07-02 16:46_

---

_@zanieb reviewed on 2025-07-02 16:47_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/environment.rs`:80 on 2025-07-02 16:47_

Just wondering, why `sys_prefix` instead of just `sys_executable` of the non-base interpreter?

---

_@oconnor663 reviewed on 2025-07-02 17:18_

---

_Review comment by @oconnor663 on `crates/uv/src/commands/project/environment.rs`:80 on 2025-07-02 17:18_

For that to work, we'd have to avoid canonicalizing it, right? It didn't feel right to use an un-canonicalized path in a cache key, but my intuition about this stuff is still quite weak, so I'll definitely do whichever feels better to you.

---

_@zanieb reviewed on 2025-07-02 17:20_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/environment.rs`:80 on 2025-07-02 17:20_

Ah that's an interesting point. It's probably fine either way, that makes sense though.

---

_Renamed from "Use sys_prefix instead of the canonicalized sys_executable in CachedEnvironment keys" to "Includes `sys.prefix` in cached environment keys to avoid `--with` collisions across projects" by @zanieb on 2025-07-02 17:47_

---

_Comment by @oconnor663 on 2025-07-02 19:36_

Here's re-running @tjni's repro from the original ticket:

```
mkdir p1
mkdir p2

cat > p1/pyproject.toml <<EOF
[project]
name = "p1"
version = "0.0.0"
dependencies = ["aiofiles"]
EOF

cat > p2/pyproject.toml <<EOF
[project]
name = "p2"
version = "0.0.0"
dependencies = ["requests"]
EOF

cat > main.py <<EOF
import sys

print(f"Python is {sys.executable}")
EOF

mkdir build
mkdir build/p1
mkdir build/p2

pushd p1
UV_PROJECT_ENVIRONMENT=../build/p1 uv sync
UV_PROJECT_ENVIRONMENT=../build/p1 uv run --no-sync --with pip ../main.py
popd

pushd p2
UV_PROJECT_ENVIRONMENT=../build/p2 uv sync
UV_PROJECT_ENVIRONMENT=../build/p2 uv run --no-sync --with pip ../main.py
popd
```

```
...
Python is /home/jacko/.cache/uv/archive-v0/7_KzAfQN3YAC9K8jGIfCr/bin/python3
...
Python is /home/jacko/.cache/uv/archive-v0/RxWxGaxHuTMc7uLjZ6SiI/bin/python3
```

---

_Merged by @zanieb on 2025-07-02 19:40_

---

_Closed by @zanieb on 2025-07-02 19:40_

---

_Branch deleted on 2025-07-02 19:40_

---
