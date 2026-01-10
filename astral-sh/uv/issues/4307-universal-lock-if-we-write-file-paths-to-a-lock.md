---
number: 4307
title: "universal-lock: if we write file paths to a lock file, we need to make them portable"
type: issue
state: closed
author: BurntSushi
labels:
  - preview
assignees: []
created_at: 2024-06-13T15:26:26Z
updated_at: 2024-06-14T13:09:18Z
url: https://github.com/astral-sh/uv/issues/4307
synced_at: 2026-01-10T01:23:36Z
---

# universal-lock: if we write file paths to a lock file, we need to make them portable

---

_Issue opened by @BurntSushi on 2024-06-13 15:26_

While working on #4266, I realized that the file paths we write to a lock file are platform specific. Specifically, on Windows, we use `\` as a path separator, while on Unix, we use `/` as a path separator. I believe this implies that if developers on Windows and Unix work on the same project, they will generate different lock files. This is, I believe, untenable.

I think we either need to find a way to not write file paths to a lock file at all (this is what Cargo does), or we need to ensure we normalize all file paths into a portable representation.

It's not clear to me that the latter is strictly possible in all cases, but I think we are always writing Unix paths to `requirements.txt` by either converting `\` to `/`, or using `file` URLs.

---

_Referenced in [astral-sh/uv#3347](../../astral-sh/uv/issues/3347.md) on 2024-06-13 15:26_

---

_Comment by @zanieb on 2024-06-13 15:29_

See also https://github.com/astral-sh/uv/pull/3804

---

_Comment by @charliermarsh on 2024-06-13 19:22_

In the interim, should we just call `to_slash` on the paths? That library exists for this purpose IIRC.

---

_Label `preview` added by @charliermarsh on 2024-06-14 03:59_

---

_Referenced in [astral-sh/uv#4324](../../astral-sh/uv/pulls/4324.md) on 2024-06-14 04:14_

---

_Comment by @BurntSushi on 2024-06-14 13:09_

Closed by #4324.

We should perhaps still explore the design space of "don't write paths to a lock file," but Charlie makes the point that this would require reading the `pyproject.toml` in order to understand the lock file. I believe, at present, the lock file is "hermetic," which could be conceivably useful (and it is at least conceptually simpler). My _very vague_ thinking on the matter at present is the following:

1. I wouldn't be surprised if we wind up needing to read `pyproject.toml` and give up the hermetic lock file approach for some other reason. But I don't know what that reason is yet.
2. I have a nagging suspicion that even by converting paths to Unix paths, there are still some cases that we'll get wrong in the sense that it isn't portable. But I'm finding it difficult to articulate those cases since, even if the paths aren't in the lock file, they have to be somewhere (like `pyproject.toml`, which needs to be valid UTF-8 and thus inherently already prevents some kinds of file paths from being expressible as-is).

---

_Closed by @BurntSushi on 2024-06-14 13:09_

---
