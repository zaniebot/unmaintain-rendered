```yaml
number: 17376
title: "dependencies: switch from `chrono` to `jiff`"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/jiff
created_at: 2025-04-13T22:24:38Z
updated_at: 2025-04-15T11:47:57Z
url: https://github.com/astral-sh/ruff/pull/17376
synced_at: 2026-01-12T15:56:01Z
```

# dependencies: switch from `chrono` to `jiff`

---

_@BurntSushi_

We weren't really using `chrono` for anything other than getting the
current time and formatting it for logs. So the switch to Jiff here
is pretty uninteresting.

Unfortunately, this doesn't quite get us to a point where `chrono`
can be removed. From what I can tell, we're still bringing it via
[`tracing-subscriber`](https://docs.rs/tracing-subscriber/latest/tracing_subscriber/)
and
[`quick-junit`](https://docs.rs/quick-junit/latest/quick_junit/).
`tracing-subscriber` does have an
[issue open about Jiff](https://github.com/tokio-rs/tracing/discussions/3128),
but there's no movement on it.

Normally I'd suggest holding off on this since it doesn't get us all of
the way there and it would be better to avoid bringing in two datetime
libraries, but we are, it appears, already there. In particular,
`env_logger` brings in Jiff. So this PR doesn't really make anything
worse, but it does bring us closer to an all-Jiff world.


---

_Review requested from @carljm by @BurntSushi on 2025-04-13 22:24_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-04-13 22:24_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-04-13 22:24_

---

_Review requested from @sharkdp by @BurntSushi on 2025-04-13 22:24_

---

_Review requested from @dcreager by @BurntSushi on 2025-04-13 22:24_

---

_Review request for @dcreager removed by @BurntSushi on 2025-04-13 22:24_

---

_Review request for @carljm removed by @BurntSushi on 2025-04-13 22:24_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-04-13 22:24_

---

_Review request for @AlexWaygood removed by @BurntSushi on 2025-04-13 22:24_

---

_Comment by @github-actions[bot] on 2025-04-13 22:26_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-04-13 22:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@carljm approved on 2025-04-14 22:38_

---

_Merged by @BurntSushi on 2025-04-15 11:47_

---

_Closed by @BurntSushi on 2025-04-15 11:47_

---

_Branch deleted on 2025-04-15 11:47_

---
