```yaml
number: 10091
title: Respect runtime-required decorators for function signatures
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/runtime-required
created_at: 2024-02-23T03:24:32Z
updated_at: 2024-02-23T03:38:35Z
url: https://github.com/astral-sh/ruff/pull/10091
synced_at: 2026-01-12T15:55:31Z
```

# Respect runtime-required decorators for function signatures

---

_@charliermarsh_

## Summary

The original implementation of this applied the runtime-required context to definitions _within_ the function, but not the signature itself. (We had test coverage; the snapshot was just correctly showing the wrong outcome.)

Closes https://github.com/astral-sh/ruff/issues/10089.


---

_Label `bug` added by @charliermarsh on 2024-02-23 03:24_

---

_Merged by @charliermarsh on 2024-02-23 03:33_

---

_Closed by @charliermarsh on 2024-02-23 03:33_

---

_Branch deleted on 2024-02-23 03:33_

---

_Comment by @github-actions[bot] on 2024-02-23 03:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (error)</summary>
<p>

```
Failed to clone rotki/rotki: error: RPC failed; curl 92 HTTP/2 stream 0 was not closed cleanly: CANCEL (err 8)
error: 5497 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
