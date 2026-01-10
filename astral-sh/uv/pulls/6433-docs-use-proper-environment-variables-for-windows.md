```yaml
number: 6433
title: "docs: Use proper environment variables for Windows"
type: pull_request
state: merged
author: T-256
labels:
  - documentation
assignees: []
merged: true
base: main
head: localappdata
created_at: 2024-08-22T13:28:37Z
updated_at: 2024-08-23T18:04:47Z
url: https://github.com/astral-sh/uv/pull/6433
synced_at: 2026-01-10T13:09:51Z
```

# docs: Use proper environment variables for Windows

---

_Pull request opened by @T-256 on 2024-08-22 13:28_

ref: https://github.com/astral-sh/uv/issues/6397#issuecomment-2304512872

---

_@zanieb reviewed on 2024-08-22 15:33_

---

_Review comment by @zanieb on `docs/concepts/cache.md`:107 on 2024-08-22 15:33_

Why this change? We're talking about uv here, e.g., "uv will instead need to fallback to slow copy operations"

---

_@zanieb reviewed on 2024-08-22 15:33_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:424 on 2024-08-22 15:33_

Would be you be willing to update all of the `FOLDERID_` references? I think there are some more.

You'll need to update the generated reference with `cargo dev generate-all`

---

_Label `documentation` added by @zanieb on 2024-08-22 15:34_

---

_Assigned to @zanieb by @zanieb on 2024-08-22 15:34_

---

_@T-256 reviewed on 2024-08-22 18:07_

---

_Review comment by @T-256 on `docs/concepts/cache.md`:107 on 2024-08-22 18:07_

by reading `fallback`, initially I thought it will automatically do it for us, because imo it means: if some progress failed do X instead.

---

_@zanieb reviewed on 2024-08-22 18:13_

---

_Review comment by @zanieb on `docs/concepts/cache.md`:107 on 2024-08-22 18:13_

It does / should do it automatically for you?

---

_@T-256 reviewed on 2024-08-22 18:32_

---

_Review comment by @T-256 on `docs/concepts/cache.md`:107 on 2024-08-22 18:32_

Then I think Warning message could be improved to mention which mode it fallbacking, in #6397 it says:
```
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
```
Instead:
```
         Cache and target directories are on different filesystems, hardlinking not supported, switched to copy mode.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
```

---

_Renamed from "docs: `%LOCALAPPDATA%` for cache dir refs" to "docs: Use proper environment vairabils for Windows" by @T-256 on 2024-08-22 18:37_

---

_Renamed from "docs: Use proper environment vairabils for Windows" to "docs: Use proper environment variables for Windows" by @konstin on 2024-08-23 08:11_

---

_@zanieb approved on 2024-08-23 18:03_

Thanks!

---

_Merged by @zanieb on 2024-08-23 18:04_

---

_Closed by @zanieb on 2024-08-23 18:04_

---

_@zanieb reviewed on 2024-08-23 18:04_

---

_Review comment by @zanieb on `docs/concepts/cache.md`:107 on 2024-08-23 18:04_

Probably worth discussion elsewhere — we have too many issues to go through right now to discuss unrelated changes out of context.

---
