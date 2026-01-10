```yaml
number: 21294
title: "Add option to provide a reason to `--add-noqa`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - cli
assignees: []
merged: true
base: main
head: claude/ruff-issue-10070-011CUrj8kV8ZWw6okJFrW76t
created_at: 2025-11-06T14:35:24Z
updated_at: 2025-11-11T13:03:48Z
url: https://github.com/astral-sh/ruff/pull/21294
synced_at: 2026-01-10T16:53:55Z
```

# Add option to provide a reason to `--add-noqa`

---

_Pull request opened by @MichaReiser on 2025-11-06 14:35_

## Summary

This PR adds an optional reason to `--add-noqa` that will be added to every inserted `noqa` comment. 

How to use:

```
ruff check --add-noqa "Disallow usage of method"
```

Fixes https://github.com/astral-sh/ruff/issues/10070


## Test Plan

Added CLI tests


---

_Label `cli` added by @MichaReiser on 2025-11-06 14:35_

---

_Comment by @github-actions[bot] on 2025-11-07 21:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @MichaReiser on 2025-11-07 23:38_

---

_Review comment by @ntBre on `crates/ruff/src/args.rs`:414 on 2025-11-10 18:14_

Do we want `require_equals = true`? I usually prefer not to use the `=` (like in the PR summary), but I'm fine either way.

---

_@ntBre approved on 2025-11-10 18:20_

---

_@MichaReiser reviewed on 2025-11-11 11:06_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:414 on 2025-11-11 11:06_

We might have to but I'm curious to hear your opinion too. I, honestly, didn't realize that claude added it until you pointed it out. So I tried to remove it but then run into issues when using `--add-noqa` with stdin filenames:

```
ruff check --add-noqa -
```

clap no incorrectly parses the `-` as the noqa comment. 

Or running

```
ruff cehck --add-noqa file.py
```

Will use `file.py` as the noqa reason rather than only adding noqa comments to `file.py`. That's why I think we should keep it as is.

---

_@ntBre reviewed on 2025-11-11 12:59_

---

_Review comment by @ntBre on `crates/ruff/src/args.rs`:414 on 2025-11-11 12:59_

Ah that makes sense! Seems fine as-is then.

---

_Merged by @MichaReiser on 2025-11-11 13:03_

---

_Closed by @MichaReiser on 2025-11-11 13:03_

---

_Branch deleted on 2025-11-11 13:03_

---
