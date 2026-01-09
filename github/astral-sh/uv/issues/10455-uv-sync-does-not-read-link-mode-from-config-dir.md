---
number: 10455
title: "`uv sync` does not read `link-mode` from  `$CONFIG_DIR/uv.toml`"
type: issue
state: closed
author: nikvaessen
labels:
  - question
assignees: []
created_at: 2025-01-10T03:19:01Z
updated_at: 2025-01-10T03:35:49Z
url: https://github.com/astral-sh/uv/issues/10455
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv sync` does not read `link-mode` from  `$CONFIG_DIR/uv.toml`

---

_Issue opened by @nikvaessen on 2025-01-10 03:19_

My `uv.toml` at `~/.config/uv/uv.toml`:

```yaml
cache-dir = "/path/to/network-attached-file-system"

[pip]
link-mode = "symlink"
```

When running `uv sync` and `uv sync --link-mode hardlink` I get this error:

```
warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
```
When running `uv sync --link-mode symlink` or `UV_LINK_MODE=symlink uv sync` everything runs as expected.
Also note that `uv add $PACKAGE` reads from `uv.toml` as expected. 

I am running `uv 0.5.16`

---

_Comment by @charliermarsh on 2025-01-10 03:20_

Settings under `[pip]` are only respected in `uv pip` commands. Remove the `[pip]` from your `uv.toml` to have it affect `uv sync`.

---

_Comment by @nikvaessen on 2025-01-10 03:22_

Thanks, that indeed fixes the issue. Would it make sense to update https://docs.astral.sh/uv/reference/settings/#pip_link-mode accordingly? Is there a benefit to including `[pip]`?

---

_Comment by @charliermarsh on 2025-01-10 03:35_

That's actually intentional. The non-`[pip]` versions are above: https://docs.astral.sh/uv/reference/settings/#link-mode. We keep the `[pip]` versions for compatibility -- we can change the non-`[pip]` versions over time at our discretion, but the `[pip]` versions are intended to mirror `pip` itself.

---

_Closed by @charliermarsh on 2025-01-10 03:35_

---

_Label `question` added by @charliermarsh on 2025-01-10 03:35_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-10 03:35_

---
