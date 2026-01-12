```yaml
number: 6140
title: "Warn when `--upgrade` is passed to `tool run`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/warn-tool-run
created_at: 2024-08-16T03:21:06Z
updated_at: 2024-08-19T17:08:40Z
url: https://github.com/astral-sh/uv/pull/6140
synced_at: 2026-01-12T16:07:14Z
```

# Warn when `--upgrade` is passed to `tool run`

---

_@charliermarsh_

## Summary

Passing `--upgrade` to `tool run` is confusing, because it doesn't upgrade the installed tool. It just causes us to use an isolated tool environment, which seems wrong.


---

_Review requested from @zanieb by @charliermarsh on 2024-08-16 03:21_

---

_Label `cli` added by @charliermarsh on 2024-08-16 03:21_

---

_Label `preview` added by @charliermarsh on 2024-08-16 03:21_

---

_@charliermarsh reviewed on 2024-08-16 03:21_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/run.rs`:427 on 2024-08-16 03:21_

I mean... should `--reinstall` get the same treatment?

---

_@zanieb reviewed on 2024-08-16 13:11_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:427 on 2024-08-16 13:11_

Yeah I don't think these flags really make sense here?

---

_@charliermarsh reviewed on 2024-08-16 17:15_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/run.rs`:427 on 2024-08-16 17:15_

It does have _some_ effect though. `--reinstall` would cause us to reinstall all packages in the _cached_ environment.

---

_@charliermarsh reviewed on 2024-08-16 17:15_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/run.rs`:427 on 2024-08-16 17:15_

(`--upgrade` makes less sense there, because we resolve without any existing preferences etc.)

---

_@zanieb reviewed on 2024-08-16 18:06_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:427 on 2024-08-16 18:06_

I feel like the cached environment shouldn't be mutable to the user — it should be all or nothing with `--refresh` covering a reset of it or `--isolated` bypassing it?

---

_@zanieb reviewed on 2024-08-16 18:07_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:427 on 2024-08-16 18:07_

Basically, I think we should treat the cached item as immutable. If we need to update it, we should discard it.

---

_@charliermarsh reviewed on 2024-08-16 20:00_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/run.rs`:427 on 2024-08-16 20:00_

How should the user indicate that it should be re-created?

---

_@zanieb reviewed on 2024-08-16 20:25_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:427 on 2024-08-16 20:25_

`--refresh`, I think.

---

_Merged by @charliermarsh on 2024-08-19 17:08_

---

_Closed by @charliermarsh on 2024-08-19 17:08_

---

_Branch deleted on 2024-08-19 17:08_

---
