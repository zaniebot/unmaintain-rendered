```yaml
number: 5156
title: "Skip invalid tools in `uv tool list`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/l
created_at: 2024-07-17T17:55:00Z
updated_at: 2024-07-18T17:56:41Z
url: https://github.com/astral-sh/uv/pull/5156
synced_at: 2026-01-12T16:06:40Z
```

# Skip invalid tools in `uv tool list`

---

_@charliermarsh_

## Summary

Makes the `tools()` return value include per-tool errors. This makes it easy to skip (rather than failing) in `uv tool list`, _and_ improves `uv tool uninstall` to remove those invalid tools, rather than leaving them around. (We already had that behavior for `uv tool uninstall ruff` with an invalid `ruff`, but `uv tool uninstall --all` just left them.)

Closes https://github.com/astral-sh/uv/issues/5151.


---

_Renamed from "Skip invalid tools in uv tool list" to "Skip invalid tools in `uv tool list`" by @charliermarsh on 2024-07-17 17:55_

---

_Label `bug` added by @charliermarsh on 2024-07-17 17:55_

---

_Label `preview` added by @charliermarsh on 2024-07-17 17:55_

---

_Marked ready for review by @charliermarsh on 2024-07-17 17:55_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-17 17:55_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:46 on 2024-07-18 04:03_

```suggestion
                "Ignoring malformed tool `{name}` (use `{}` to remove)",
```

---

_@zanieb reviewed on 2024-07-18 04:03_

---

_@zanieb reviewed on 2024-07-18 04:03_

---

_Review comment by @zanieb on `crates/uv/tests/tool_list.rs`:87 on 2024-07-18 04:03_

```suggestion
    warning: Ignoring malformed tool `black` (use `uv tool uninstall black` to remove)
```

---

_@zanieb reviewed on 2024-07-18 04:03_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/uninstall.rs`:74 on 2024-07-18 04:03_

"IO" :)

---

_@zanieb approved on 2024-07-18 04:05_

---

_@charliermarsh reviewed on 2024-07-18 14:06_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/list.rs`:46 on 2024-07-18 14:06_

It's green though so I intentionally omitted this, what do you think?

---

_@zanieb reviewed on 2024-07-18 14:27_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:46 on 2024-07-18 14:27_

Oh. I don't think the color is enough alone (and it's not great for users with colorblindness or colors turned off).


---

_Merged by @charliermarsh on 2024-07-18 17:56_

---

_Closed by @charliermarsh on 2024-07-18 17:56_

---

_Branch deleted on 2024-07-18 17:56_

---
