---
number: 13609
title: "Feature Request: Enable shared caching or linking for heavy binary wheels like jaxlib across projects"
type: issue
state: closed
author: mav3ri3k
labels:
  - question
assignees: []
created_at: 2025-05-23T02:47:22Z
updated_at: 2025-05-24T12:40:33Z
url: https://github.com/astral-sh/uv/issues/13609
synced_at: 2026-01-10T01:25:35Z
---

# Feature Request: Enable shared caching or linking for heavy binary wheels like jaxlib across projects

---

_Issue opened by @mav3ri3k on 2025-05-23 02:47_

### Summary

I'm using `uv` across multiple Python projects (especially for JAX-related experiments), and I noticed that every time I create a new virtual environment, `uv` reinstalls large binary wheels like `jaxlib` from scratch. These wheels (e.g., `jaxlib`, `torch`, `tensorflow`, etc.) can be several hundred megabytes in size — resulting in gigabytes of redundant storage across `.venv` folders.

In my case, `jaxlib` + `xla_extension.so` alone account for 400MB+, and I have several near-identical `.venv/` directories across sibling projects. This adds up quickly.


### Example

### Expected Behavior

A way to:
- Share or symlink installed binary wheels across environments
- Optionally create venvs that **reuse a shared store** of heavy native extensions

This could be similar to:
- `pnpm`'s node_modules symlinking strategy

### Possible Solutions

- Add a `--shared-site-packages` or `--symlink-packages` flag

### Why this matters

- Significant **disk space savings** for users working with large Python libraries

### Environment

- `uv` version: 0.4.25 (97eb6ab4a 2024-10-21)
- OS: macOS
- Python version: 3.10
- Project types: ML/Scientific computing (JAX, torch, etc.)

---

_Label `enhancement` added by @mav3ri3k on 2025-05-23 02:47_

---

_Label `enhancement` removed by @konstin on 2025-05-23 08:19_

---

_Label `question` added by @konstin on 2025-05-23 08:19_

---

_Comment by @konstin on 2025-05-23 08:20_

On macOS, uv uses reflinks (via clonefile), so the copies are virtual and shared across all venv created from the cache.

---

_Comment by @Scxw010516 on 2025-05-23 18:54_

> On macOS, uv uses reflinks (via clonefile), so the copies are virtual and shared across all venv created from the cache

What is the behavior like on Windows? Will it create a complete new copy? Or can it be reused through links? 

---

_Comment by @konstin on 2025-05-23 19:04_

This is documented under https://docs.astral.sh/uv/reference/cli/#uv-sync--link-mode

---

_Comment by @Scxw010516 on 2025-05-23 19:27_

> This is documented under https://docs.astral.sh/uv/reference/cli/#uv-sync--link-mode

Thanks for your reply. Does this mean that when I install packages in any project, it will first look for them in the global cache? If the package already exists in the .venv of another project, it will create a hardlink?

---

_Comment by @konstin on 2025-05-23 19:32_

yes, uv always goes through the cache when installing packages, so a package is added to the cache once and then reuse through linking.

---

_Comment by @Scxw010516 on 2025-05-23 19:41_

> yes, uv always goes through the cache when installing packages, so a package is added to the cache once and then reuse through linking.

Even if I use different versions of Python in different virtual environments from different Conda env, is it still the case?

---

_Comment by @konstin on 2025-05-23 19:48_

We don't make any specific guarantees for how the caching behaves, but I can say that uv is optimized for cache reuse and that the wheel caching is based on the specific wheel that gets installed, not on the target environment.

---

_Comment by @Scxw010516 on 2025-05-23 19:52_

> We don't make any specific guarantees for how the caching behaves, but I can say that uv is optimized for cache reuse and that the wheel caching is based on the specific wheel that gets installed, not on the target environment.

Thanks for your great work.

---

_Comment by @mav3ri3k on 2025-05-24 10:14_

> This is documented under https://docs.astral.sh/uv/reference/cli/#uv-sync--link-mode

Thanks. `clone` and `hardlink` were copying the entire thing, maybe my file system does not support those. `symlink` did work, and I have made it my default. 
Hopefully uv can auto-magically decide the most efficient option.

---

_Closed by @mav3ri3k on 2025-05-24 10:14_

---

_Comment by @charliermarsh on 2025-05-24 11:21_

How are you verifying that they were copying the entire file? I think that's very unlikely -- they might show up with `du -h` or similar because those commands aren't aware of hardlink sharing, but I'd **strongly** recommend sticking with hardlinks or copy-on-write links over symlinks.

---

_Comment by @mav3ri3k on 2025-05-24 12:40_

You are spot on. I was using `dust`, but results were same with `du` too. Thanks for the recommendation, I'll take your word for it and use the default - clone. Thanks! 👍

---
