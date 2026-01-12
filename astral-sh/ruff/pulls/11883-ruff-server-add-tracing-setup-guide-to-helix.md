```yaml
number: 11883
title: "`ruff server`: Add tracing setup guide to Helix documentation "
type: pull_request
state: merged
author: snowsignal
labels:
  - documentation
  - server
assignees: []
merged: true
base: main
head: jane/server/docs/hx/trace
created_at: 2024-06-14T22:16:40Z
updated_at: 2024-06-18T03:41:25Z
url: https://github.com/astral-sh/ruff/pull/11883
synced_at: 2026-01-12T15:55:39Z
```

# `ruff server`: Add tracing setup guide to Helix documentation 

---

_@snowsignal_

A follow-up to [this suggestion](https://github.com/astral-sh/ruff/pull/11747#discussion_r1634297757) on the tracing PR.

---

_Label `documentation` added by @snowsignal on 2024-06-14 22:16_

---

_Label `server` added by @snowsignal on 2024-06-14 22:16_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-06-15 08:09_

---

_Review request for @dhruvmanila removed by @MichaReiser on 2024-06-15 08:09_

---

_Review comment by @MichaReiser on `crates/ruff_server/docs/setup/HELIX.md`:64 on 2024-06-15 08:10_

The second part feels redundant IMO
```suggestion
to either `messages` or `verbose`.
```

---

_@MichaReiser approved on 2024-06-15 08:11_

Thanks. 

Does ruff log the messages to the default location in helix? Do users know where to find the logs?

---

_Review comment by @dhruvmanila on `crates/ruff_server/docs/setup/HELIX.md`:98 on 2024-06-17 06:22_

nit: it might be useful to provide a path suggestion here to the directory for Helix's log file. I'm not sure whether Helix expands the `~` (tilde) character to the home directory, you might want to confirm this if you accept this suggestion

```suggestion
logFile = "~/.cache/helix/ruff.log"
```

---

_@dhruvmanila approved on 2024-06-17 06:23_

---

_Comment by @snowsignal on 2024-06-17 16:40_

@MichaReiser
> Does ruff log the messages to the default location in helix? Do users know where to find the logs?

Yes, by default it logs them to the Helix log file, which can be opened with `:log-open`.

---

_@snowsignal reviewed on 2024-06-18 03:36_

---

_Review comment by @snowsignal on `crates/ruff_server/docs/setup/HELIX.md`:98 on 2024-06-18 03:36_

I've tested this, and we don't expand `~` at the moment. I'll make a follow-up issue for this.

---

_Merged by @snowsignal on 2024-06-18 03:41_

---

_Closed by @snowsignal on 2024-06-18 03:41_

---

_Branch deleted on 2024-06-18 03:41_

---
