```yaml
number: 22530
title: "[ty] Improve log guidance message for Zed"
type: pull_request
state: open
author: dhruvmanila
labels:
  - server
assignees: []
base: main
head: dhruv/log-guidance
created_at: 2026-01-12T09:31:56Z
updated_at: 2026-01-13T05:57:25Z
url: https://github.com/astral-sh/ruff/pull/22530
synced_at: 2026-01-13T06:27:11Z
```

# [ty] Improve log guidance message for Zed

---

_@dhruvmanila_

## Summary

closes: https://github.com/astral-sh/ty/issues/1717

## Test Plan

https://github.com/user-attachments/assets/5d8d6d03-3451-4403-a6cd-2deba9e796fc




---

_Marked ready for review by @dhruvmanila on 2026-01-12 12:48_

---

_Review requested from @carljm by @dhruvmanila on 2026-01-12 12:48_

---

_Review requested from @MichaReiser by @dhruvmanila on 2026-01-12 12:48_

---

_Review requested from @sharkdp by @dhruvmanila on 2026-01-12 12:48_

---

_Review requested from @dcreager by @dhruvmanila on 2026-01-12 12:48_

---

_Review requested from @Gankra by @dhruvmanila on 2026-01-12 12:48_

---

_Review request for @dcreager removed by @dhruvmanila on 2026-01-12 12:49_

---

_Review request for @carljm removed by @dhruvmanila on 2026-01-12 12:49_

---

_Review request for @sharkdp removed by @dhruvmanila on 2026-01-12 12:49_

---

_Label `server` added by @dhruvmanila on 2026-01-12 12:49_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api.rs`:126 on 2026-01-12 17:44_

Should we make `session.client` an `enum` instead of mapping at the use sides? 

```rust
#[derive(Debug, Clone)]
enum Client {
	Zed,
	Unknown(String)
}
```

---

_Review comment by @MichaReiser on `crates/ty_server/src/session.rs`:1152 on 2026-01-12 17:46_

```suggestion
            "Check the logs for more details (command palette: `dev: open language server logs`)."
```

The *find logs* part confused me for a bit. I think saying "command pallete" and mentioning the text to search for should be sufficient.

---

_@MichaReiser approved on 2026-01-12 17:47_

I'd prefer mapping the client to an enum early (e.g. in `Server`) rather than doing it in the code path using the name

---

_@dhruvmanila reviewed on 2026-01-13 05:56_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api.rs`:126 on 2026-01-13 05:56_

I've made this change except:
1. Used `ClientName` because there's already a `Client` struct
2. Used `Other` instead of `Unknown` without storing the `String` for now, we can add it if it's needed

---
