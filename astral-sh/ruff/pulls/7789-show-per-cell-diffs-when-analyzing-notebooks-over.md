```yaml
number: 7789
title: "Show per-cell diffs when analyzing notebooks over `stdin`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: charlie/diff
created_at: 2023-10-03T18:40:01Z
updated_at: 2023-10-04T14:05:26Z
url: https://github.com/astral-sh/ruff/pull/7789
synced_at: 2026-01-12T02:39:10Z
```

# Show per-cell diffs when analyzing notebooks over `stdin`

---

_Pull request opened by @charliermarsh on 2023-10-03 18:40_

## Summary

The implementation here differs from the non-`stdin` version -- this is now more consistent.

## Test Plan

```
❯ cat Untitled.ipynb | cargo run -p ruff_cli -- check --stdin-filename Untitled.ipynb --diff -n
    Finished dev [unoptimized + debuginfo] target(s) in 0.11s
     Running `target/debug/ruff check --stdin-filename Untitled.ipynb --diff -n`
--- Untitled.ipynb:cell 2
+++ Untitled.ipynb:cell 2
@@ -1 +0,0 @@
-import os
--- Untitled.ipynb:cell 4
+++ Untitled.ipynb:cell 4
@@ -1 +0,0 @@
-import sys
```


---

_Review requested from @dhruvmanila by @charliermarsh on 2023-10-03 18:40_

---

_Label `bug` added by @charliermarsh on 2023-10-03 18:40_

---

_Label `cli` added by @charliermarsh on 2023-10-03 18:40_

---

_Comment by @codspeed-hq[bot] on 2023-10-03 18:53_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/diff)

### Merging #7789 will **improve performances by 2.58%**

<sub>Comparing <code>charlie/diff</code> (f35e4f1) with <code>main</code> (600471e)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/diff` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 39 ms | 38 ms | +2.58% |


---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/diagnostics.rs`:432 on 2023-10-04 02:22_

nit: This is repeated for stdout as well but I think the only difference is that the path is optional. Could we combine them under `SourceKind` in the same way as done in `FixMode::Apply`? That would avoid code duplication.

---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/diagnostics.rs`:496 on 2023-10-04 02:25_

Do you think a default value of `-` (or `stdin` / `STDIN`) would be good to show as the filename for stdin? I think we do that in other places. So, the output would something like:
```
-:cell 0:
...

-:cell 2:
...
```

---

_@dhruvmanila approved on 2023-10-04 02:25_

Thanks!

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:496 on 2023-10-04 13:48_

Hmm, we don't do this in the Python case, so I'll leave it for now.

---

_@charliermarsh reviewed on 2023-10-04 13:48_

---

_@charliermarsh reviewed on 2023-10-04 13:48_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:432 on 2023-10-04 13:48_

I'll take a look at this in a follow-up as its leading me to want to make some other changes.

---

_Merged by @charliermarsh on 2023-10-04 13:58_

---

_Closed by @charliermarsh on 2023-10-04 13:58_

---

_Branch deleted on 2023-10-04 13:58_

---

_Comment by @github-actions[bot] on 2023-10-04 14:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
