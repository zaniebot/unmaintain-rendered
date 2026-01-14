```yaml
number: 17101
title: "add ```--no-frozen``` flag to commands for flexible lockfile management "
type: pull_request
state: closed
author: VictorHuu
labels:
  - no-build
assignees: []
base: main
head: victorh/add-no-frozen
created_at: 2025-12-12T13:47:29Z
updated_at: 2026-01-14T17:10:22Z
url: https://github.com/astral-sh/uv/pull/17101
synced_at: 2026-01-14T17:38:04Z
```

# add ```--no-frozen``` flag to commands for flexible lockfile management 

---

_@VictorHuu_

fixes #17044

## Summary

Note that ```--frozen``` can't be used with ```--locked```, this PR introduces a ```--no-frozen``` CLI flag to several commands to make it possible to bypass the ```UV_FROZEN``` environment variable or config settings on a per-command basis. 
## Test Plan
Ensures that `--no-frozen` and `--frozen` can override each other
> Generally, in the same context, cases of `--frozen` take precedence. Otherwise, we need to create a new context.

Ensures that `--no-frozen` can be used with `--locked`


---

_@zanieb reviewed on 2025-12-12 13:50_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:710 on 2025-12-12 13:50_

Shouldn't we resolve this early? We usually do so in `settings.rs` why does this happen in each of the commands?

---

_Label `no-build` added by @zanieb on 2025-12-12 13:52_

---

_Marked ready for review by @VictorHuu on 2025-12-13 10:41_

---

_Converted to draft by @VictorHuu on 2025-12-14 04:00_

---

_Marked ready for review by @VictorHuu on 2025-12-15 15:08_

---

_Renamed from "add --no-frozen flag to commands for flexible lockfile management " to "add ```--no-frozen``` flag to commands for flexible lockfile management " by @VictorHuu on 2025-12-15 16:10_

---

_Review requested from @zanieb by @VictorHuu on 2025-12-16 12:49_

---

_Comment by @VictorHuu on 2025-12-16 14:42_

Hi @zanieb ! Could you please review this tiny bug fix when you are available? As a newbie, I don't know much about the details of contributions , so any review will be helpful:)

---

_Comment by @AndreuCodina on 2025-12-29 06:54_

Is everything ok with this PR? :)

---

_Comment by @VictorHuu on 2025-12-30 11:31_

@AndreuCodina still waiting for review now. CC @zanieb 

---

_Closed by @VictorHuu on 2026-01-14 17:10_

---
