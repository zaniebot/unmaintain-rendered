```yaml
number: 4647
title: uv fails to start because of jemalloc
type: issue
state: closed
author: adminy
labels: []
assignees: []
created_at: 2024-06-29T16:38:43Z
updated_at: 2024-08-26T16:49:24Z
url: https://github.com/astral-sh/uv/issues/4647
synced_at: 2026-01-12T15:58:51Z
```

# uv fails to start because of jemalloc

---

_@adminy_

```
<jemalloc>: Unsupported system page size
<jemalloc>: Unsupported system page size
memory allocation of 5 bytes failed
[1]    22202 abort (core dumped)  uv
```

### A minimal code snippet that reproduces the bug.
`uv venv` or any uv command.
### The current uv platform.
[NixOS derivation](https://github.com/NixOS/nixpkgs/blob/master/pkgs/by-name/uv/uv/package.nix) running on raspberry Pi latest kernel with the aarch64 target.
### The current uv version (`uv --version`).
0.2.15 ( I think, not sure because of the error)


---

_Comment by @zanieb on 2024-06-29 17:03_

Looks like jemalloc is not supported on that operating system e.g. https://github.com/home-assistant/core/issues/105768

Home-assistant provides a `DISABLE_JEMALLOC` option to work-around this, we might need to do similar?

It looks like they also changed the page size (i.e. `JEMALLOC_SYS_WITH_LG_PAGE`) which may solve the issue? https://github.com/home-assistant/docker-base/pull/248 and more discussion at https://github.com/qdrant/qdrant/pull/3945#issuecomment-2030026387

---

_Comment by @konstin on 2024-07-01 07:13_

This is the same problem as we had in https://github.com/astral-sh/ruff/issues/3791, we can try the same mitigation (`JEMALLOC_SYS_WITH_LG_PAGE=16` during build)

---

_Label `bug` added by @konstin on 2024-07-01 07:13_

---

_Comment by @charliermarsh on 2024-07-01 12:25_

I believe we have that same mitigation in our build already @konstin.

---

_Comment by @konstin on 2024-07-01 12:30_

This looks like a bug in the nixos derivation then, which doesn't set `JEMALLOC_SYS_WITH_LG_PAGE=16` for the aarch64 target.

@adminy can you confirm that uv works with the official package (either from github or pypi)?

---

_Label `bug` removed by @konstin on 2024-07-09 10:24_

---

_Comment by @zanieb on 2024-08-26 16:49_

I'm going to close this as stale. #6528 tracks this issue for our own binaries.

---

_Closed by @zanieb on 2024-08-26 16:49_

---
