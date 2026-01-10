```yaml
number: 16686
title: "[ruff] Fix `last_tag`/`commits_since_last_tag` for `version` command"
type: pull_request
state: merged
author: ZedThree
labels:
  - cli
assignees: []
merged: true
base: main
head: fix-last-tag-distance
created_at: 2025-03-12T17:20:32Z
updated_at: 2025-03-13T12:05:19Z
url: https://github.com/astral-sh/ruff/pull/16686
synced_at: 2026-01-10T19:49:02Z
```

# [ruff] Fix `last_tag`/`commits_since_last_tag` for `version` command

---

_Pull request opened by @ZedThree on 2025-03-12 17:20_

## Summary

Since Ruff changed to GitHub releases, tags are no longer annotated and `git describe` no longer picks them up. Instead, it's necessary to also search lightweight tags.

This changes fixes the `version` command to give more accurate `last_tag`/`commits_since_last_tag` information. This only affects development builds, as this information is not present in releases.

## Test Plan

Testing is a little tricky because this information changes on every commit. Running manually on current `main` and my branch:

`main`:

```
# cargo run --bin ruff -- version --output-format=text
ruff 0.9.10+2547 (dd2313ab0 2025-03-12)

# cargo run --bin ruff -- version --output-format=json
{
  "version": "0.9.10",
  "commit_info": {
    "short_commit_hash": "dd2313ab0",
    "commit_hash": "dd2313ab0faea90abf66a75f1b5c388e728d9d0a",
    "commit_date": "2025-03-12",
    "last_tag": "v0.4.10",
    "commits_since_last_tag": 2547
  }
}
```

This PR:

```
# cargo run --bin ruff -- version --output-format=text
ruff 0.9.10+46 (11f39f616 2025-03-12)

# cargo run --bin ruff -- version --output-format=json
{
  "version": "0.9.10",
  "commit_info": {
    "short_commit_hash": "11f39f616",
    "commit_hash": "11f39f6166c3d7a521725b938a166659f64abb59",
    "commit_date": "2025-03-12",
    "last_tag": "0.9.10",
    "commits_since_last_tag": 46
  }
}
```

---

_Comment by @github-actions[bot] on 2025-03-12 17:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @zanieb by @MichaReiser on 2025-03-12 17:49_

---

_Label `cli` added by @MichaReiser on 2025-03-12 17:49_

---

_@MichaReiser approved on 2025-03-13 11:59_

Thanks this is great! We may need the same fix in uv (@zanieb)

---

_Merged by @MichaReiser on 2025-03-13 11:59_

---

_Closed by @MichaReiser on 2025-03-13 11:59_

---

_Branch deleted on 2025-03-13 12:03_

---

_Comment by @ZedThree on 2025-03-13 12:05_

Looks like it's already been fixed in uv: https://github.com/astral-sh/uv/commit/94bec44dadeb43cf274e75c8eccc1c2979f6c33c

---
