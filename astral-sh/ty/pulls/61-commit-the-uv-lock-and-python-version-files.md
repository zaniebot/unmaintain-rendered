```yaml
number: 61
title: "Commit the `uv.lock` and `.python-version` files"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/uv-files
created_at: 2025-05-06T13:35:11Z
updated_at: 2025-07-08T10:38:16Z
url: https://github.com/astral-sh/ty/pull/61
synced_at: 2026-01-12T15:54:27Z
```

# Commit the `uv.lock` and `.python-version` files

---

_@zanieb_

These are just in my tree right now, but seem proper to commit.

---

_Marked ready for review by @zanieb on 2025-05-06 13:35_

---

_@MichaReiser approved on 2025-05-06 14:06_

Would adding them to `.gitignore` be an alternative, considering that we don't have any dependencies or "active" python version.

---

_Comment by @zanieb on 2025-05-06 14:31_

uv will use a consistent Python version when running scripts, which is helpful. We should also probably use `dependency-groups`, which would make the `uv.lock` relevant. e.g., we could replace the uvx `rooster` invocation such that it actually uses locked dependencies. 

---

_Comment by @zanieb on 2025-05-06 14:31_

(I think it's best practice to use these, not ignore them — and I don't think our use-case is an exception in that regard)

---

_Comment by @AlexWaygood on 2025-05-06 14:35_

Adding the `.python-version` file makes sense to me. Does the `uv.lock` file make sense? When I'm running scripts from the Ruff repo, I nearly always use `uv run --no-project`, because if I don't then uv tries to install Ruff from source using maturin (which takes forever, because Rust builds are so slow :P )

---

_Comment by @zanieb on 2025-05-06 14:44_

> Does the uv.lock file make sense? When I'm running scripts from the Ruff repo, I nearly always use uv run --no-project, because if I don't then uv tries to install Ruff from source using maturin (which takes forever, because Rust builds are so slow :P )

Well, I set up cache keys for that already (#39) so the rebuilds are less out of control but still slow. However, for dependency groups, you'd do or `--only-group <name>` and we'll skip building the project:

```
❯ uv run --only-dev  python -c "print('hi')"
hi
```

So... yes, the `uv.lock` makes sense — and we should use dependency groups (at the very least, for Rooster).

---

_Comment by @AlexWaygood on 2025-05-06 14:45_

OK, sounds good!

---

_Merged by @zanieb on 2025-05-06 14:45_

---

_Closed by @zanieb on 2025-05-06 14:45_

---

_Comment by @zanieb on 2025-05-06 14:50_

Moving rooster over there in #64, oh glorious locked dependencies :D

---

_Branch deleted on 2025-07-08 10:38_

---
