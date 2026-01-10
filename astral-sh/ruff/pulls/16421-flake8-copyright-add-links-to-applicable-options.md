```yaml
number: 16421
title: "[`flake8-copyright`] Add links to applicable options (`CPY001`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - documentation
assignees: []
merged: true
base: main
head: CPY001
created_at: 2025-02-27T20:23:04Z
updated_at: 2025-02-28T10:19:12Z
url: https://github.com/astral-sh/ruff/pull/16421
synced_at: 2026-01-10T19:49:01Z
```

# [`flake8-copyright`] Add links to applicable options (`CPY001`)

---

_Pull request opened by @InSyncWithFoo on 2025-02-27 20:23_

## Summary

Resolves #16420.

`CPY001`'s documentation now links to all three options in the [`lint.flake8-copyright`](https://docs.astral.sh/ruff/settings/#lintflake8-copyright) section.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @InSyncWithFoo on 2025-02-27 20:26_

Between these three separate links and a single link to `lint.flake8-copyright` itself, which would be better from an UX perspective?

I think it's the latter, but there don't seem to be any other "group" links in the entire codebase.

---

_Comment by @github-actions[bot] on 2025-02-27 20:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @ntBre on 2025-02-27 23:25_

---

_Comment by @MichaReiser on 2025-02-28 08:11_

Thanks for doing this! 

I like what you did here. It's a bit more repetitive but users don't need to do one extra click to see if any of the options might be relevant for them.

---

_Merged by @MichaReiser on 2025-02-28 08:11_

---

_Closed by @MichaReiser on 2025-02-28 08:11_

---

_Branch deleted on 2025-02-28 10:19_

---
