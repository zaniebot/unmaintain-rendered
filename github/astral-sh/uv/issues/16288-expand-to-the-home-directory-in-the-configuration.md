---
number: 16288
title: "Expand `~` to the home directory in the configuration files"
type: issue
state: open
author: leodevian
labels:
  - enhancement
assignees: []
created_at: 2025-10-13T20:41:43Z
updated_at: 2025-12-09T01:06:04Z
url: https://github.com/astral-sh/uv/issues/16288
synced_at: 2026-01-07T13:12:19-06:00
---

# Expand `~` to the home directory in the configuration files

---

_Issue opened by @leodevian on 2025-10-13 20:41_

### Summary

Path entries in the configuration file are not resolved, making it hard to write and share standard `uv.toml` configuration files.

On Windows, using the following `uv.toml` to provide user-level configuration would create directories named `~` in whatever directory uv was called from while it needed its cache.

```toml
cache-dir = "~/.cache/uv"
```

Expanding `~` to the home directory would enable to make universal, shareable configuration files.

Furthermore, I doubt that `~` is used for anything other than referencing home directories.

### Example


```toml
cache-dir = "~/.cache/uv"
```

This user-level or system-level `uv.toml` would make uv create its cache in the home directory of the user.

---

_Label `enhancement` added by @leodevian on 2025-10-13 20:41_

---

_Renamed from "Resolve `~` as the home directory in the configuration files" to "Expand `~` to the home directory in the configuration files" by @leodevian on 2025-10-13 20:46_

---

_Comment by @GlowingScrewdriver on 2025-11-06 07:47_

I second this, would be really nice to have.

> On Windows, ... would create directories named ~ ...

I can confirm that this is the case on Linux too.

---

_Comment by @Mobocop on 2025-12-05 20:55_

I came here to report this. However I think it is a bug and not an enhancement. When testing this on Linux (Ubuntu 24.04, uv 0.9.15) and setting the `cache-dir` in my local uv.toml I have found that uv does use the cache directory specified (e.g. `~/.cache/uv`), however it also creates an empty folder called `~` in the current directory.

---

_Comment by @Mobocop on 2025-12-05 22:44_

https://github.com/astral-sh/uv/blob/c269619b1bc9d2db10a76f3af2ece2bf56ef9ba2/crates/uv-cache/src/cli.rs#L43-L48

I think `cache_dir` needs to be expanded for tildes, maybe in a similar way to [how it is done in parts of ruff?](https://github.com/astral-sh/ruff/blob/ef45c97dab2e1ab8268308a53e6dd452db40b0db/crates/ruff_server/src/logging.rs#L18-L29)


---

_Comment by @Mobocop on 2025-12-07 21:25_

Having tested this on windows it looks like it's forcing the cache-dir to be in a folder called `~`, whereas on linux it makes a folder called `~` but then uses the cache-dir specified. I'll open a PR for my fix.

---

_Referenced in [astral-sh/uv#17019](../../astral-sh/uv/pulls/17019.md) on 2025-12-07 21:56_

---

_Comment by @konstin on 2025-12-08 14:23_

Can you expand about why the default cache dir and it's more general configuration options (XDG paths) don't work for you?

---

_Comment by @GlowingScrewdriver on 2025-12-08 17:03_

> Can you expand about why the default cache dir and it's more general configuration options (XDG paths) don't work for you?

So, my situation is a bit wonky 😬 . My `~/Desktop/`, where I typically keep my working trees, is on a separate disk partition from `~/`. Since hardlinks cannot work across partitions, I wanted to configure UV to keep its cache under `~/Desktop/`.

An alternative setup (apart from hardcoding my home directory) could be to expose some subdirectory of `~/Desktop/` under `~/.cache/uv`, via perhaps a bind-mount or a symlink. But that is arguably less elegant.

---

_Comment by @leodevian on 2025-12-09 01:03_

> Can you expand about why the default cache dir and it's more general configuration options (XDG paths) don't work for you?

I'm maintaining the configuration of uv within my organization and I'm using `uv.toml` for Windows environments as downloading it and moving it to the standard directory is understandable to all users.

We cannot disable the maximum path length limitation on Windows machines and I'm just trying to save as many characters as possible...

To be fair, I was mostly suprised that uv created `~` directories. I believe that my error can be reproduced by many other people, thus the issue. 

---
