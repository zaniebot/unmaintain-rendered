```yaml
number: 12208
title: "Consider `--preview` flag for `server` subcommand"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: main
head: dhruv/server-preview
created_at: 2024-07-05T14:00:36Z
updated_at: 2024-07-18T05:35:03Z
url: https://github.com/astral-sh/ruff/pull/12208
synced_at: 2026-01-12T15:55:40Z
```

# Consider `--preview` flag for `server` subcommand

---

_@dhruvmanila_

## Summary

This PR removes the requirement of `--preview` flag to run the `ruff server` and instead considers it to be an indicator to turn on preview mode for the linter and the formatter.

resolves: #12161 

## Test Plan

Add test cases to assert the `preview` value is updated accordingly.

In an editor context, I used the local `ruff` executable in Neovim with the `--preview` flag and verified that the preview-only violations are being highlighted.

Running with:
```lua
require('lspconfig').ruff.setup({
  cmd = {
    '/Users/dhruv/work/astral/ruff/target/debug/ruff',
    'server',
    '--preview',
  },
})
```
The screenshot shows that `E502` is highlighted with the below config in `pyproject.toml`:

<img width="877" alt="Screenshot 2024-07-17 at 16 43 09" src="https://github.com/user-attachments/assets/c7016ef3-55b1-4a14-bbd3-a07b1bcdd323">


---

_Label `server` added by @dhruvmanila on 2024-07-05 14:00_

---

_Comment by @github-actions[bot] on 2024-07-05 14:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dhruvmanila on 2024-07-09 07:40_

(This is in draft because it also removes the requirement of the `--preview` flag for the native server, so should be merged when the server is stabilized.)

---

_Marked ready for review by @dhruvmanila on 2024-07-17 10:19_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-07-17 10:19_

---

_@MichaReiser approved on 2024-07-17 11:20_

---

_Merged by @dhruvmanila on 2024-07-18 05:35_

---

_Closed by @dhruvmanila on 2024-07-18 05:35_

---

_Branch deleted on 2024-07-18 05:35_

---
