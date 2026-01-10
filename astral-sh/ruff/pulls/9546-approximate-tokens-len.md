```yaml
number: 9546
title: Approximate tokens len
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: approximate-tokens-len
created_at: 2024-01-16T08:11:45Z
updated_at: 2024-01-19T16:39:38Z
url: https://github.com/astral-sh/ruff/pull/9546
synced_at: 2026-01-10T22:57:09Z
```

# Approximate tokens len

---

_Pull request opened by @MichaReiser on 2024-01-16 08:11_

## Summary

The `parser` benchmark has become flaky after merging https://github.com/astral-sh/ruff/pull/9466. 
A possible reason for the flakiness could be some non-determinism in jemalloc that makes it execute different branches while collecting the tokens into a `Vec`. 

The idea of this PR is to approximate the size of the tokens `Vec` based on the source code length. The approximation used here is based on an analysis of the CPython code base and our ecosystem projects (including aiflow, black, django, transformers, twine, warehouse, zulip). I added a console output that prints the source code length and resulting `tokens` vec size for each file and piped it into a CSV. I then used a small python script to paint the $buffer/source$ distribution and picked 0.15 as a lower bound for the size of the tokens vec. 

**Ecosystem**
![ecosystem](https://github.com/astral-sh/ruff/assets/1203881/19c82944-69dd-4c9a-87ef-4382fe95f5c2)

**CPython**
![python](https://github.com/astral-sh/ruff/assets/1203881/66c322a3-a705-4be1-9032-0be6b37b8a19)


> **Note**: The 1 looks suspicious. It is due to empty files. I'm not sure why there are so many empty files


[Source](https://gist.github.com/MichaReiser/5a75f2d4ce66fb9c59b5a3ab2aa685c3)



## Test Plan

I expected some real world perf improvements from this work but neither our `hyperfine` benchmarks nor the parser's microbenchmarks show any real improvement :shrug: 


---

_Marked ready for review by @MichaReiser on 2024-01-16 08:46_

---

_Label `internal` added by @MichaReiser on 2024-01-16 08:46_

---

_Comment by @github-actions[bot] on 2024-01-16 08:52_

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

_@charliermarsh approved on 2024-01-16 20:22_

Neat idea.

---

_Merged by @MichaReiser on 2024-01-19 16:39_

---

_Closed by @MichaReiser on 2024-01-19 16:39_

---

_Branch deleted on 2024-01-19 16:39_

---
