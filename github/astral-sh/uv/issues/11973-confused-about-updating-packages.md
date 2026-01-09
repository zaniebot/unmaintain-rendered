---
number: 11973
title: Confused about updating packages
type: issue
state: open
author: lucharo
labels:
  - question
assignees: []
created_at: 2025-03-05T09:15:16Z
updated_at: 2025-03-10T21:56:17Z
url: https://github.com/astral-sh/uv/issues/11973
synced_at: 2026-01-07T13:12:18-06:00
---

# Confused about updating packages

---

_Issue opened by @lucharo on 2025-03-05 09:15_

### Question

Hi, the other day I was working on my project, managed by uv. I wanted to upgrade my polars version to the latest version. I ran `uv add -U polars` from a folder not in the root of my project (so `pyproject.toml`) wasn't in that folder, the environment updated succesfully but the changes weren't reflected in my `pyproject.toml`). I then navigated to the root of the project and ran the same command again, still nothing happened. Then I ran `uv add "polars==1.24.0"` and finally the `pyproject.toml`, is this expected? 

I would have assumed that `uv add -U polars` would have updated my environment and my `pyproject.toml` from wherever it had been executed. 



### Platform

Darwin 24.3.0 arm64

### Version

uv 0.5.4 (c62c83c37 2024-11-20)

---

_Label `question` added by @lucharo on 2025-03-05 09:15_

---

_Comment by @zanieb on 2025-03-05 19:03_

Sounds like you're looking for https://github.com/astral-sh/uv/issues/6794

It does sound reasonable to bump the lower bound in `uv add -U polars` though 🤔 

cc @konstin

---

_Comment by @sglbl on 2025-03-10 16:41_

Related [11509](https://github.com/astral-sh/uv/issues/11509) . I agree and I thought the same thing that this already existed.

---

_Comment by @konstin on 2025-03-10 21:56_

Agreed that this is unintuitive.

For context, this is currently happening because we treat `--upgrade` as a (basically global) lockfile-only option, so `uv add --upgrade foo` says "add foo to pyproject.toml (if it doesn't exist) and update the versions in the lockfile" instead of the expected "add _the latest lower bound_ of foo to pyproject.toml and update the versions in the lockfile".

---
