```yaml
number: 10625
title: Avoid parsing the root configuration twice
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: avoid-parsing-root-configuration-twice
created_at: 2024-03-27T09:20:53Z
updated_at: 2024-05-01T09:32:51Z
url: https://github.com/astral-sh/ruff/pull/10625
synced_at: 2026-01-10T22:37:01Z
```

# Avoid parsing the root configuration twice

---

_Pull request opened by @MichaReiser on 2024-03-27 09:20_

## Summary
I debugged #10622 and noticed that the root configuration is always resolved twice. This PR changes `python_files_in_path` to stop searching for configurations if it reached the root-configuration directory (because we can't find any more specific configuration).

## Test Plan

I added debug statements to verify that `into_settings` is no longer called twice for the root configuration. 

I see a 4% speedup when running the CPython all cached benchmarks with the default rule selection

```
hyperfine --warmup 10 --runs 20 \
        "./target/release/ruff-main check crates/ruff_linter/resources/test/cpython -e" \
        "./target/release/ruff check crates/ruff_linter/resources/test/cpython -e"
Benchmark 1: ./target/release/ruff-main check crates/ruff_linter/resources/test/cpython -e
  Time (mean ± σ):      37.1 ms ±   1.1 ms    [User: 43.6 ms, System: 43.6 ms]
  Range (min … max):    35.1 ms …  39.4 ms    20 runs
 
Benchmark 2: ./target/release/ruff check crates/ruff_linter/resources/test/cpython -e
  Time (mean ± σ):      35.3 ms ±   0.7 ms    [User: 36.7 ms, System: 51.0 ms]
  Range (min … max):    34.0 ms …  36.4 ms    20 runs
 
Summary
  ./target/release/ruff check crates/ruff_linter/resources/test/cpython -e ran
    1.05 ± 0.04 times faster than ./target/release/ruff-main check crates/ruff_linter/resources/test/cpython -e
```


---

_Comment by @github-actions[bot] on 2024-03-27 09:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/registry/rule_set.rs`:211 on 2024-03-27 09:49_

This is kind of unrelated but I noticed that we can simplify `configuration.rs` a bit

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/configuration.rs`:1100 on 2024-03-27 09:49_

Another unrelated config change. It avoids allocating a new `Vec` and instead reuses the existing vector.

---

_@MichaReiser reviewed on 2024-03-27 09:49_

---

_Label `internal` added by @MichaReiser on 2024-03-27 09:49_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-03-27 09:49_

---

_@charliermarsh approved on 2024-03-27 14:11_

---

_Marked ready for review by @MichaReiser on 2024-05-01 09:20_

---

_Merged by @MichaReiser on 2024-05-01 09:28_

---

_Closed by @MichaReiser on 2024-05-01 09:28_

---

_Branch deleted on 2024-05-01 09:28_

---
