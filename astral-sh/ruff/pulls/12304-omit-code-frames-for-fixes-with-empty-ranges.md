```yaml
number: 12304
title: Omit code frames for fixes with empty ranges
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/frame
created_at: 2024-07-12T13:19:16Z
updated_at: 2024-07-12T15:30:41Z
url: https://github.com/astral-sh/ruff/pull/12304
synced_at: 2026-01-10T21:47:02Z
```

# Omit code frames for fixes with empty ranges

---

_Pull request opened by @charliermarsh on 2024-07-12 13:19_

## Summary

Closes https://github.com/astral-sh/ruff/issues/12291.

## Test Plan

```shell
❯ cargo run check ../uv/foo --select INP
/Users/crmarsh/workspace/uv/foo/bar/baz.py:1:1: INP001 File `/Users/crmarsh/workspace/uv/foo/bar/baz.py` is part of an implicit namespace package. Add an `__init__.py`.
Found 1 error.
```


---

_Label `cli` added by @charliermarsh on 2024-07-12 13:19_

---

_Review requested from @dhruvmanila by @charliermarsh on 2024-07-12 13:19_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/message/text.rs`:119 on 2024-07-12 13:45_

I think we should probably restrict it to `0..0` as this will also catch any `n..n` where `n` > 0.

```suggestion
				// The `0..0` range is used to highlight file-level diagnostics.
                if message.range() != TextRange::default() {
```

---

_@dhruvmanila approved on 2024-07-12 13:45_

---

_@zanieb approved on 2024-07-12 14:45_

Nice!

---

_Merged by @charliermarsh on 2024-07-12 15:21_

---

_Closed by @charliermarsh on 2024-07-12 15:21_

---

_Branch deleted on 2024-07-12 15:21_

---

_Comment by @adamtheturtle on 2024-07-12 15:22_

The responsiveness of Astral is incredible. Thanks all!

---

_Comment by @github-actions[bot] on 2024-07-12 15:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
