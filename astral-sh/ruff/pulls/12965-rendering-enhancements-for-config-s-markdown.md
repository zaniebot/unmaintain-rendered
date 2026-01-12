```yaml
number: 12965
title: "Rendering enhancements for `config`'s Markdown output "
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - cli
assignees: []
base: main
head: main
created_at: 2024-08-18T17:19:13Z
updated_at: 2024-08-18T17:56:56Z
url: https://github.com/astral-sh/ruff/pull/12965
synced_at: 2026-01-12T15:55:42Z
```

# Rendering enhancements for `config`'s Markdown output 

---

_@InSyncWithFoo_

Fixes #12960.

I updated the tests accordingly and also added a "deprecated" test case which uses `lint.extend-ignore`. There might be a better way to write those trailing spaces, however.

---

_Renamed from "Enforce line breaks in `config`'s Markdown output" to "Rendering enhancements for `config`'s Markdown output " by @InSyncWithFoo on 2024-08-18 17:24_

---

_Comment by @github-actions[bot] on 2024-08-18 17:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-08-18 17:48_

Thanks for posting this. I looked more closely at the command, and the output is explicitly text, not markdown.

I think I would rather support adding a `--output-format=markdown` option rather than changing the text format because it won't be obvious for anyone coming across this code in the future also to test that the output is valid markdown (and that we support it in the first place)

An explicit markdown output format also allows us to optimize the output by e.g using paragraphs or bullet lists. 

---

_Label `cli` added by @MichaReiser on 2024-08-18 17:50_

---

_Comment by @InSyncWithFoo on 2024-08-18 17:56_

Fair enough. I'll close this PR then.

---

_Closed by @InSyncWithFoo on 2024-08-18 17:56_

---
