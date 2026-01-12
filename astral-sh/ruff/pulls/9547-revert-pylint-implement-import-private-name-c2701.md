```yaml
number: 9547
title: "Revert \"[`pylint`] Implement `import-private-name` (`C2701`)\""
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
assignees: []
merged: true
base: main
head: revert-5920-import-private-name
created_at: 2024-01-16T08:28:35Z
updated_at: 2024-01-16T10:01:35Z
url: https://github.com/astral-sh/ruff/pull/9547
synced_at: 2026-01-12T15:55:29Z
```

# Revert "[`pylint`] Implement `import-private-name` (`C2701`)"

---

_@MichaReiser_

Reverts astral-sh/ruff#5920 because It's panicking in the pydantic benchmark run (see [astral-sh/ruff/actions/runs/7537305800/job/20516059364?pr=5920](https://github.com/astral-sh/ruff/actions/runs/7537305800/job/20516059364?pr=5920)).

```
ruff on  lexer-explicit-id-false-brnaches [$] is 📦 v0.1.13 via 🐍 v3.11.6 via 🦀 v1.75.0 
❯ cargo bench -p ruff_benchmark --bench linter --  "all-with-preview-rules[pydantic/types.py]"
    Finished bench [optimized] target(s) in 0.08s
     Running benches/linter.rs (target/release/deps/linter-3478ab4e9ed35ca9)
linter/all-with-preview-rules/numpy/globals.py
                        time:   [165.16 µs 165.62 µs 166.37 µs]
                        thrpt:  [17.736 MiB/s 17.816 MiB/s 17.866 MiB/s]
Found 6 outliers among 100 measurements (6.00%)
  1 (1.00%) high mild
  5 (5.00%) high severe
linter/all-with-preview-rules/unicode/pypinyin.py
                        time:   [657.40 µs 657.83 µs 658.47 µs]
                        thrpt:  [6.3813 MiB/s 6.3875 MiB/s 6.3917 MiB/s]
Found 11 outliers among 100 measurements (11.00%)
  6 (6.00%) high mild
  5 (5.00%) high severe
linter/all-with-preview-rules/pydantic/types.py
                        time:   [2.5630 ms 2.5687 ms 2.5749 ms]
                        thrpt:  [9.9046 MiB/s 9.9285 MiB/s 9.9506 MiB/s]
Found 20 outliers among 100 measurements (20.00%)
  18 (18.00%) high mild
  2 (2.00%) high severe
Benchmarking linter/all-with-preview-rules/numpy/ctypeslib.py: Warming up for 3.0000 sthread 'main' panicked at crates/ruff_linter/src/rules/pylint/rules/import_private_name.rs:137:41:
range end index 2 out of range for slice of length 1
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

error: bench failed, to rerun pass `-p ruff_benchmark --bench linter`
```

@tjkuson would you mind having a look? I think it's due to the [slicing into `module_name`](https://github.com/astral-sh/ruff/pull/5920/files/bd483d3fc35fef88afddd7313ed64df142caa038#diff-deb7d79fa4cb9ffa3d1a73320007e23db5022916746a287d8bf70b2da5c9c01d)

---

_Label `rule` added by @MichaReiser on 2024-01-16 08:28_

---

_Merged by @MichaReiser on 2024-01-16 08:33_

---

_Closed by @MichaReiser on 2024-01-16 08:33_

---

_Branch deleted on 2024-01-16 08:33_

---

_Comment by @codspeed-hq[bot] on 2024-01-16 08:35_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/revert-5920-import-private-name)

### Merging #9547 will **improve performances by 4.66%**

<sub>:warning: No base runs were found</sub>

<sub>Falling back to comparing <code>revert-5920-import-private-name</code> (b045ee1) with <code>main</code> (7ef7e0d)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `revert-5920-import-private-name` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[numpy/ctypeslib.py]` | 12.8 ms | 12.2 ms | +4.66% |


---

_Comment by @github-actions[bot] on 2024-01-16 08:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @tjkuson on 2024-01-16 09:30_

@MichaReiser I can submit a patch this evening, sorry for any disruption!

---

_Comment by @MichaReiser on 2024-01-16 10:01_

> @MichaReiser I can submit a patch this evening, sorry for any disruption!

No worries at all and thanks for taking a look. This is also not urgent (that's why I reverted, there's no need to rush it)

---
