---
number: 7433
title: "`uv add --branch` is broken solely on a particular repo"
type: issue
state: closed
author: Xmaster6y
labels:
  - bug
  - great writeup
assignees: []
created_at: 2024-09-16T17:11:10Z
updated_at: 2024-09-17T02:21:43Z
url: https://github.com/astral-sh/uv/issues/7433
synced_at: 2026-01-10T01:24:15Z
---

# `uv add --branch` is broken solely on a particular repo

---

_Issue opened by @Xmaster6y on 2024-09-16 17:11_

## Problem

`uv` doesn't resolve the good commit when using `--branch <branch>` but `@<branch>` does (the repo: https://github.com/Xmaster6y/vec_noise)

When I use:
```
uv add git+https://github.com/Xmaster6y/vec_noise@refacto 
```

I get (good rev):

```
Updated https://github.com/Xmaster6y/vec_noise (1e281ec)
Updated https://github.com/Xmaster6y/vec_noise (1e281ec)
```

But when I use:
```
uv add git+https://github.com/Xmaster6y/vec_noise --branch refacto
```

I get:

```
Updated https://github.com/Xmaster6y/vec_noise (7c3865d)
...
```

### Same for `--rev`

Using
```
uv add git+https://github.com/Xmaster6y/vec_noise --rev 1e281ec
```
gives :
```
Updated https://github.com/Xmaster6y/vec_noise (7c3865d)
...
```

### Specificities

- The repository is a second order fork
- The original repository is fairly old -> a fork belongs to the original repository network

## What didn't work

- I tried to add from a branch on a fork -> worked as expected
- I tried to add from a branch on a second order fork -> worked as expected

## Details

- Plateform: MacOS (Darwin 23.5.0 arm64)
- `uv` version: uv 0.4.10 (690716484 2024-09-13)


---

_Label `bug` added by @zanieb on 2024-09-16 17:13_

---

_Label `great writeup` added by @zanieb on 2024-09-16 17:13_

---

_Comment by @zanieb on 2024-09-16 17:13_

Thank you for the well-written report!

---

_Comment by @zanieb on 2024-09-16 17:17_

As a note, `7c3865d` is the head commit on `main` and the upstream fork (but not the original repository)

---

_Comment by @Xmaster6y on 2024-09-16 17:21_

Puzzling edit:

I previously add:

```
uv add git+https://github.com/astral-sh/ruff --branch sys-version-info
```

giving:

```
 Updated https://github.com/astral-sh/ruff (bb12fe9d0)
 Updated https://github.com/astral-sh/ruff (61a5a86a7)
 ...
 ```
 
 But now:
 
 ```
uv add git+https://github.com/astral-sh/ruff --branch sys-version-info
```

giving:

```
 Updated https://github.com/astral-sh/ruff (bb12fe9d0)
 ...
 ```

---

_Comment by @zanieb on 2024-09-16 17:28_

The difference in behavior appears to be because of https://github.com/astral-sh/uv/blob/cafc1f986a6efda0c2f384e63930333a812c9ea4/crates/uv/src/commands/project/add.rs#L241-L242

We only pass the `requirements` in here and when you use the `--branch` syntax the source is tracked separately from the package requirement.

```
[crates/uv/src/commands/project/add.rs:240:5] &requirements = [
    Package(
        "git+https://github.com/Xmaster6y/vec_noise",
    ),
]
```


---

_Comment by @Xmaster6y on 2024-09-16 17:32_

Is this wanted/expected?


---

_Comment by @Xmaster6y on 2024-09-16 17:33_

My later observation seems unrelated to my first issue

This happen when my `uv.lock` is not sync.

### To reproduce:

Run (should break, otherwise interupt it mid-course):
```
uv add git+https://github.com/astral-sh/ruff --branch sys-version-info 
```

Run the command again (not calling `uv sync/lock`):

```
uv add git+https://github.com/astral-sh/ruff --branch sys-version-info 
```

Does it deserve another issue?

---

_Comment by @zanieb on 2024-09-16 17:33_

We later fail at

https://github.com/astral-sh/uv/blob/cafc1f986a6efda0c2f384e63930333a812c9ea4/crates/uv/src/commands/project/add.rs#L316-L324 

If you name the dependency, the correct revision is resolved

```
❯ uv add vec_noise@git+https://github.com/Xmaster6y/vec_noise --branch refacto
 Updated https://github.com/Xmaster6y/vec_noise (1e281ec)
```

> Is this wanted/expected?

No, I don't think the behavior we're seeing is wanted or expected. I'm just debugging and sharing some notes.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-16 17:40_

---

_Referenced in [astral-sh/uv#7447](../../astral-sh/uv/pulls/7447.md) on 2024-09-17 02:06_

---

_Closed by @charliermarsh on 2024-09-17 02:21_

---

_Closed by @charliermarsh on 2024-09-17 02:21_

---
