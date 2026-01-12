```yaml
number: 2469
title: Use prek in documentation and CI
type: pull_request
state: open
author: MatthewMckee4
labels: []
assignees: []
base: main
head: use-prek
created_at: 2026-01-12T16:06:19Z
updated_at: 2026-01-12T16:09:41Z
url: https://github.com/astral-sh/ty/pull/2469
synced_at: 2026-01-12T16:13:29Z
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

I tried to see if adding priority for each hook, but it has almost no effect.

```text
> $ hyperfine --parameter-list branch main,use-prek \                                                                                                                     
--setup "git switch {branch}" \
--warmup 3  \
"prek run --all-files"
Benchmark 1: prek run --all-files (branch = main)
  Time (mean ± σ):      4.475 s ±  0.305 s    [User: 5.546 s, System: 0.664 s]
  Range (min … max):    4.126 s …  4.975 s    10 runs

Benchmark 2: prek run --all-files (branch = use-prek)
  Time (mean ± σ):      4.566 s ±  0.389 s    [User: 5.514 s, System: 0.672 s]
  Range (min … max):    4.102 s …  5.103 s    10 runs

Summary
  prek run --all-files (branch = main) ran
    1.02 ± 0.11 times faster than prek run --all-files (branch = use-prek)
``` 

---
