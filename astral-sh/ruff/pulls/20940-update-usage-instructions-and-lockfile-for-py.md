```yaml
number: 20940
title: Update usage instructions and lockfile for py-fuzzer script
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: alex/lock-fuzzer
created_at: 2025-10-17T14:10:56Z
updated_at: 2025-10-17T14:57:18Z
url: https://github.com/astral-sh/ruff/pull/20940
synced_at: 2026-01-10T17:34:34Z
```

# Update usage instructions and lockfile for py-fuzzer script

---

_Pull request opened by @AlexWaygood on 2025-10-17 14:10_

## Summary

Various improvements to how we document and execute the py-fuzzer script:

- Recommend using `uv run --project` rather than `uvx --from`, and use that command in our CI worfklows. This means that the lockfile at `./python/py-fuzzer/uv.lock` will be respected, reducing the chances of a repeat of https://github.com/astral-sh/ruff/pull/20161#issuecomment-3238304922. It also means that you don't need to pass `--reinstall` to avoid uv caching the script (which would mean it would ignore updates you made to it locally).
- Also use `--locked` in our CI workflows, because why not
- Use Python ~~3.14~~ 3.13 for the `daily_fuzz.yaml` workflow
- Bump dependency pins in the lockfile

## Test Plan

CI on this PR


---

_Label `ci` added by @AlexWaygood on 2025-10-17 14:10_

---

_@MichaReiser approved on 2025-10-17 14:12_

---

_Comment by @github-actions[bot] on 2025-10-17 14:31_

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

_Marked ready for review by @AlexWaygood on 2025-10-17 14:57_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-10-17 14:57_

---

_Merged by @AlexWaygood on 2025-10-17 14:57_

---

_Closed by @AlexWaygood on 2025-10-17 14:57_

---

_Branch deleted on 2025-10-17 14:57_

---
