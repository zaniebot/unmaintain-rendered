```yaml
number: 19252
title: "[ty] Ecosystem analyzer: parallelize, fix race condition"
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/update-ecosystem-analyzer
created_at: 2025-07-10T07:59:44Z
updated_at: 2025-07-10T08:25:25Z
url: https://github.com/astral-sh/ruff/pull/19252
synced_at: 2026-01-12T15:56:35Z
```

# [ty] Ecosystem analyzer: parallelize, fix race condition

---

_@sharkdp_

## Summary

Pulls in two fixes and a performance optimization:

- Fix a bug with the Markdown table formatting.
- Combine the two `analyze` commands into a single `diff` command. This means we only need to set up the projects once, which is faster and also avoids a race condition where projects could change between the two `analyze` runs.
- ~~Run `ty` in parallel across projects~~ (this seems to cause hangs :disappointed:)


---

_Label `ci` added by @sharkdp on 2025-07-10 07:59_

---

_Label `ty` added by @sharkdp on 2025-07-10 07:59_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-10 08:00_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-10 08:15_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-10 08:15_

---

_Comment by @github-actions[bot] on 2025-07-10 08:24_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No changes detected ✅
**[Full report with detailed diff](https://david-update-ecosystem-analy.ecosystem-663.pages.dev/diff)**


---

_Merged by @sharkdp on 2025-07-10 08:25_

---

_Closed by @sharkdp on 2025-07-10 08:25_

---

_Branch deleted on 2025-07-10 08:25_

---
