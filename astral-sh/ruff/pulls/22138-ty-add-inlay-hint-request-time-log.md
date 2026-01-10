```yaml
number: 22138
title: "[ty] Add inlay hint request time log"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: inlay-hint-lsp-time-trace
created_at: 2025-12-22T13:39:39Z
updated_at: 2025-12-27T00:21:19Z
url: https://github.com/astral-sh/ruff/pull/22138
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Add inlay hint request time log

---

_Pull request opened by @MatthewMckee4 on 2025-12-22 13:39_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This will help with testing the performance of #22111

---

_Review requested from @carljm by @MatthewMckee4 on 2025-12-22 13:39_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-12-22 13:39_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-12-22 13:39_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-12-22 13:39_

---

_Renamed from "Inlay hint lsp time trace" to "[ty] Add inlay hint request time log" by @MatthewMckee4 on 2025-12-22 13:39_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/inlay_hints.rs`:83 on 2025-12-22 14:02_

```suggestion
            "Inlay hints computed in {:?}",
```

What's the motivation for printing all inlay hints? I worry that this gets very verbose very quickly. 




---

_@MichaReiser reviewed on 2025-12-22 14:02_

We could also consider bumping the logging here to `debug`:

https://github.com/astral-sh/ruff/blob/5e943d3539e66606f860e45fbf4b2579de017e5a/crates/ty_server/src/server/main_loop.rs#L113 

---

_@MatthewMckee4 reviewed on 2025-12-22 14:06_

---

_Review comment by @MatthewMckee4 on `crates/ty_server/src/server/api/requests/inlay_hints.rs`:83 on 2025-12-22 14:06_

I just wanted to print the length to match the completion debug log.

---

_Comment by @MatthewMckee4 on 2025-12-22 14:07_

> We could also consider bumping the logging here to `debug`:

Sure, that means i could remove or change the log in completions.rs too.

---

_@MichaReiser approved on 2025-12-22 14:07_

---

_Comment by @MichaReiser on 2025-12-22 14:08_

> Sure, that means i could remove or change the log in completions.rs too.

We might want to have both. The request-level time includes the time the request spent waiting for a background worker

---

_Merged by @MichaReiser on 2025-12-22 14:08_

---

_Closed by @MichaReiser on 2025-12-22 14:08_

---

_Label `internal` added by @MichaReiser on 2025-12-22 14:08_

---

_Label `ty` added by @MichaReiser on 2025-12-22 14:08_

---

_Branch deleted on 2025-12-27 00:21_

---
