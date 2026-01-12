```yaml
number: 2469
title: Use prek in documentation and CI
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - internal
assignees: []
merged: true
base: main
head: use-prek
created_at: 2026-01-12T16:06:19Z
updated_at: 2026-01-12T17:36:13Z
url: https://github.com/astral-sh/ty/pull/2469
synced_at: 2026-01-12T18:23:18Z
```

# Use prek in documentation and CI

---

_@MatthewMckee4_

<!--
Thank you for contributing to ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

prek [now used in ruff](https://github.com/astral-sh/ruff/pull/22505), so nice if we could use it here too.

Added priority too.

```text
> $ hyperfine --parameter-list branch main,use-prek \
--setup "git switch {branch}" \
--warmup 2  --runs 3 \
"prek run --all-files"
Benchmark 1: prek run --all-files (branch = main)
  Time (mean ± σ):      3.582 s ±  0.055 s    [User: 10.782 s, System: 1.475 s]
  Range (min … max):    3.541 s …  3.644 s    3 runs

Benchmark 2: prek run --all-files (branch = use-prek)
  Time (mean ± σ):      2.651 s ±  0.014 s    [User: 12.615 s, System: 1.714 s]
  Range (min … max):    2.638 s …  2.666 s    3 runs

Summary
  prek run --all-files (branch = use-prek) ran
    1.35 ± 0.02 times faster than prek run --all-files (branch = main)
``` 

```text
> $ hyperfine 'prek run -a' 'pre-commit run --all-files' --runs 3 -i
Benchmark 1: prek run -a
  Time (mean ± σ):      2.680 s ±  0.047 s    [User: 12.638 s, System: 1.763 s]
  Range (min … max):    2.634 s …  2.729 s    3 runs

Benchmark 2: pre-commit run --all-files
  Time (mean ± σ):      4.522 s ±  0.103 s    [User: 13.003 s, System: 1.933 s]
  Range (min … max):    4.446 s …  4.639 s    3 runs

Summary
  prek run -a ran
    1.69 ± 0.05 times faster than pre-commit run --all-files
```

---

_Converted to draft by @MatthewMckee4 on 2026-01-12 16:19_

---

_Marked ready for review by @MatthewMckee4 on 2026-01-12 16:26_

---

_Label `internal` added by @MichaReiser on 2026-01-12 17:33_

---

_Comment by @MichaReiser on 2026-01-12 17:33_

I don't have a strong preference which one it is. I use them rarely enough but I strongly agree that it should be the same everywhere (I'm too old to remember which tool to use in which repository)

---

_@MichaReiser approved on 2026-01-12 17:34_

Thank you

---

_Merged by @MichaReiser on 2026-01-12 17:36_

---

_Closed by @MichaReiser on 2026-01-12 17:36_

---
