---
number: 17261
title: Is uv tool install . supposed to be caching the local folder?
type: issue
state: open
author: EternityForest
labels:
  - documentation
  - question
assignees: []
created_at: 2025-12-30T09:21:00Z
updated_at: 2026-01-06T16:57:58Z
url: https://github.com/astral-sh/uv/issues/17261
synced_at: 2026-01-10T01:26:16Z
---

# Is uv tool install . supposed to be caching the local folder?

---

_Issue opened by @EternityForest on 2025-12-30 09:21_

### Question

Redoing `uv tool install --force .` from the working directory seems to not pick up any changes, unless you use `--no-cache.`

`uv cache clean` seems to fix it, but it seems like it shouldn't be caching the local directory at all. Is this intentional?

### Platform

Linux 6.12.47+rpt-rpi-v8 aarch64 GNU/Linux

### Version

uv 0.9.20

---

_Label `question` added by @EternityForest on 2025-12-30 09:21_

---

_Comment by @EliteTK on 2025-12-30 15:13_

Whether it makes sense to use the cache or not in this case, I am not sure. But it is possible that there should have been some cache miss in your case, depending on circumstances.

Has the package's version changed since the last time you did an install? And if so, is it a dynamic version?

If both of those apply then I think, #10683 _might_ be related as I think I can hit a similar kind of bug with a dynamic version and `uv tool install`.

It might help figure out why uv picks the cached version if you produce some output with `-vv`.

As an addendum to your workaround: If you want to avoid a full cache clean in this case, you can specify the package name like `uv cache clean <name>`.

Lastly, this may not be appropriate for your use-case, but also consider `uv tool install --editable .` as an option here (see `uv help tool install` for more information).

---

_Comment by @EternityForest on 2025-12-31 12:37_

Version number hasn't changed, just the actual code, but I'd still expect local folders to bypass the cache by default, since it's not the kind of thing one typically expects to be cached unless you really know the details of how it works internally.

--editable is probably the solution in this specific case, but if caching is intentional even for the local FS(for consistency, I suppose?) then the rationale should probably documented somewhere if it isn't already, right?  Maybe the examples in the docs should mention --editable?

---

_Label `documentation` added by @EliteTK on 2025-12-31 14:34_

---

_Comment by @EliteTK on 2025-12-31 14:37_

Yeah the version number thing was just to see if you had hit a specific bug I found. I wasn't trying to imply that you should be bumping the version here. I am not sure myself and I'll defer to others for an opinion in that regard.

---

_Comment by @konstin on 2026-01-06 16:57_

Generally the `--editable` is the correct solution when testing a tool you develop. There aren't too many other cases to `uv tool install` with a path. Another option is [cache-keys](https://docs.astral.sh/uv/reference/settings/#cache-keys), especially for tools with native code, which allows much finer grained control than e.g. `--reinstall-package`.

---
