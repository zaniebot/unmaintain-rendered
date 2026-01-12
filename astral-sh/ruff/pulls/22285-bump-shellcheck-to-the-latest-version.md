```yaml
number: 22285
title: Bump shellcheck to the latest version
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/bump-shellcheck
created_at: 2025-12-29T18:12:42Z
updated_at: 2026-01-09T19:25:28Z
url: https://github.com/astral-sh/ruff/pull/22285
synced_at: 2026-01-12T15:57:45Z
```

# Bump shellcheck to the latest version

---

_@AlexWaygood_

## Summary

This dependency isn't updated by Renovate because Renovate can only update `additional_dependencies` in `.pre-commit-config.yaml` if they're additional Python dependencies for a Python pre-commit hook, or additional node dependencies for a node pre-commit hook. In this case, it's an additional Go dependency for a Go pre-commit hook (x-ref https://github.com/renovatebot/renovate/pull/39469).

Renovate should be able to update the separate shellcheck-py hook, though. Not sure why that one was so stale.

## Test Plan

`uvx prek run -a --hook-stage=manual`


---

_Label `dependencies` added by @AlexWaygood on 2025-12-29 18:12_

---

_Label `dependencies` removed by @AlexWaygood on 2025-12-29 18:21_

---

_Label `internal` added by @AlexWaygood on 2025-12-29 18:21_

---

_Merged by @AlexWaygood on 2025-12-29 18:22_

---

_Closed by @AlexWaygood on 2025-12-29 18:22_

---

_Branch deleted on 2025-12-29 18:22_

---

_Comment by @mschoettle on 2026-01-09 19:24_

@AlexWaygood I noticed the cross-references on the Renovate PR (which was merged and released today) and noticed that the required `language: golang` was missing so I went ahead and created one PR each to add it.

---

_Comment by @AlexWaygood on 2026-01-09 19:25_

thank you!!

---
