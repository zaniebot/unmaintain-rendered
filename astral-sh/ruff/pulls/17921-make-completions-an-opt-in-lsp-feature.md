```yaml
number: 17921
title: Make completions an opt-in LSP feature
type: pull_request
state: merged
author: charliermarsh
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: charlie/capabilities
created_at: 2025-05-07T15:53:39Z
updated_at: 2025-05-07T16:39:36Z
url: https://github.com/astral-sh/ruff/pull/17921
synced_at: 2026-01-12T15:56:08Z
```

# Make completions an opt-in LSP feature

---

_@charliermarsh_

## Summary

We now expect the client to send initialization options to opt-in to experimental (but LSP-standardized) features, like completion support. Specifically, the client should set `"experimental.completions.enable": true`.

Closes https://github.com/astral-sh/ty/issues/74.


---

_Review requested from @carljm by @charliermarsh on 2025-05-07 15:53_

---

_Review requested from @MichaReiser by @charliermarsh on 2025-05-07 15:53_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-05-07 15:53_

---

_Review requested from @sharkdp by @charliermarsh on 2025-05-07 15:53_

---

_Review requested from @dcreager by @charliermarsh on 2025-05-07 15:53_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-07 15:55_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:65 on 2025-05-07 16:03_

I think it's important that `init_logging` (line 53) happens *after* `init_messenger`

---

_@MichaReiser approved on 2025-05-07 16:04_

Thank you

---

_Label `server` added by @MichaReiser on 2025-05-07 16:04_

---

_Label `ty` added by @MichaReiser on 2025-05-07 16:04_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/settings.rs`:26 on 2025-05-07 16:22_

nit: `is_completions_enabled`

---

_@dhruvmanila approved on 2025-05-07 16:24_

---

_@BurntSushi approved on 2025-05-07 16:27_

Nice!

---

_@dhruvmanila reviewed on 2025-05-07 16:29_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server.rs`:65 on 2025-05-07 16:29_

Yeah, I think so too. The messenger needs to be initialized first because it's being used when serializing the initialization options. If there are invalid initialization options, it requires the messenger to provide a notification to the user.

---

_Comment by @github-actions[bot] on 2025-05-07 16:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @charliermarsh on 2025-05-07 16:39_

---

_Closed by @charliermarsh on 2025-05-07 16:39_

---

_Branch deleted on 2025-05-07 16:39_

---
