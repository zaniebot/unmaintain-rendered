```yaml
number: 13923
title: "[red-knot] knot benchmark: fix `--knot-path` arg"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/knot-benchmark-fix
created_at: 2024-10-25T09:32:41Z
updated_at: 2024-10-25T09:43:41Z
url: https://github.com/astral-sh/ruff/pull/13923
synced_at: 2026-01-12T15:55:46Z
```

# [red-knot] knot benchmark: fix `--knot-path` arg

---

_@sharkdp_

## Summary

Previously, this would fail with

```
AttributeError: 'str' object has no attribute 'is_file'
```

if I tried to use the `--knot-path` option. I wish we had a type checker for Python*.

## Test Plan

```sh
uv run benchmark --knot-path ~/.cargo-target/release/red_knot
```

\* to be fair, this would probably require special handling for `argparse` in the typechecker.

---

_Label `internal` added by @sharkdp on 2024-10-25 09:32_

---

_Label `red-knot` added by @sharkdp on 2024-10-25 09:32_

---

_@sharkdp reviewed on 2024-10-25 09:32_

---

_Review comment by @sharkdp on `scripts/knot_benchmark/src/benchmark/run.py`:72 on 2024-10-25 09:32_

This is the actual fix.

---

_Merged by @sharkdp on 2024-10-25 09:43_

---

_Closed by @sharkdp on 2024-10-25 09:43_

---

_Branch deleted on 2024-10-25 09:43_

---
