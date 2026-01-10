```yaml
number: 7509
title: uv falls back to copy instead of symlink
type: issue
state: closed
author: ypnos
labels: []
assignees: []
created_at: 2024-09-18T16:57:37Z
updated_at: 2025-05-09T14:03:56Z
url: https://github.com/astral-sh/uv/issues/7509
synced_at: 2026-01-10T03:41:46Z
```

# uv falls back to copy instead of symlink

---

_Issue opened by @ypnos on 2024-09-18 16:57_

When using a Docker cache mount, as recommended in the documentation, uv falls back to copying instead of hardlinking and complains about it. The message also recommends to suppress the complaint with explicitely setting the link mode to COPY.

See https://github.com/astral-sh/uv/blob/fe4e39a230c1d48c706a031838e76969b43cd2bf/crates/install-wheel-rs/src/linker.rs#L443

As a matter of fact, uv now supports link mode "symlink".

1. It would probably be great to try symlink before falling back to copy
2. The error message makes it sound like we have to live with the performance degradation, and does not mention that symlink mode is also available
3. The Docker documentation should include ` ENV UV_LINK_MODE=symlink` so that people do not run into this issue. See https://docs.astral.sh/uv/guides/integration/docker/#caching

---

_Comment by @charliermarsh on 2024-09-18 16:58_

Symlinks are fairly problematic, since if the cache gets deleted, your entire project will break. I generally would not want to default to them.

---

_Comment by @zanieb on 2024-09-18 17:00_

We set `copy` in the example at https://github.com/astral-sh/uv-docker-example/blob/3d2a5afd814babec58804b9ffce923ea114a76fb/Dockerfile#L10-L11 — needs to be updated over here.

---

_Comment by @notatallshaw on 2024-09-18 19:04_

> Symlinks are fairly problematic, since if the cache gets deleted, your entire project will break. I generally would not want to default to them.

I use symlink mode at work and I agree with this, I've hit an issue twice where I had to recreate my entire environment, for me it's worth the trade off but I think a user should know they are opting into symlink.

---

_Comment by @charliermarsh on 2024-09-20 03:15_

I think this is the correct behavior. It's totally fine for users to use symlinks, but they are also dangerous so I'd prefer not to make them the default.

---

_Closed by @charliermarsh on 2024-09-20 03:15_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-20 03:15_

---

_Comment by @lbsucceed on 2025-02-18 06:45_

```shell
warning: Failed to symlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, symlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
```

I encountered this problem, but I couldn't solve it. My system is Windows, and uv is installed by pip, but it works normally when I use hardlink and copy

---

_Comment by @GaussianGuaicai on 2025-05-09 14:03_

> warning: Failed to symlink files; falling back to full copy. This may lead to degraded performance.
>          If the cache and target directories are on different filesystems, symlinking may not be supported.
>          If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
> I encountered this problem, but I couldn't solve it. My system is Windows, and uv is installed by pip, but it works normally when I use hardlink and copy

```shell
warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
```

I have the same issues, even the hardlink...

---
