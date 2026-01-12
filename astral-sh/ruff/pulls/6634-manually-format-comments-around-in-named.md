```yaml
number: 6634
title: "Manually format comments around `:=` in named expressions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/format-named-expr
created_at: 2023-08-16T22:24:57Z
updated_at: 2023-08-18T03:39:12Z
url: https://github.com/astral-sh/ruff/pull/6634
synced_at: 2026-01-12T15:55:22Z
```

# Manually format comments around `:=` in named expressions

---

_@charliermarsh_

## Summary

Attaches comments around the `:=` operator in a named expression as dangling, and formats them manually in the `named_expr.rs` formatter.

Closes https://github.com/astral-sh/ruff/issues/5695.

## Test Plan

`cargo test`


---

_Label `formatter` added by @charliermarsh on 2023-08-16 22:25_

---

_Comment by @github-actions[bot] on 2023-08-16 23:08_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04      4.0±0.15ms    10.1 MB/sec    1.00      3.9±0.03ms    10.4 MB/sec
formatter/numpy/ctypeslib.py               1.03    838.5±3.42µs    19.9 MB/sec    1.00    810.7±4.21µs    20.5 MB/sec
formatter/numpy/globals.py                 1.06     87.8±0.67µs    33.6 MB/sec    1.00     82.9±0.46µs    35.6 MB/sec
formatter/pydantic/types.py                1.03  1650.7±14.93µs    15.4 MB/sec    1.00  1596.2±15.29µs    16.0 MB/sec
linter/all-rules/large/dataset.py          1.01     12.6±0.05ms     3.2 MB/sec    1.00     12.5±0.07ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.01      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    466.7±1.18µs     6.3 MB/sec    1.01    471.9±0.99µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.04ms     4.0 MB/sec    1.01      6.5±0.05ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.02ms     6.2 MB/sec    1.01      6.6±0.03ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1439.9±6.41µs    11.6 MB/sec    1.01   1456.8±3.25µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.4±0.35µs    17.6 MB/sec    1.02    170.8±0.67µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.01ms     8.7 MB/sec    1.01      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      4.6±0.27ms     8.9 MB/sec     1.01      4.6±0.40ms     8.8 MB/sec
formatter/numpy/ctypeslib.py               1.07   939.4±56.38µs    17.7 MB/sec     1.00   874.6±47.60µs    19.0 MB/sec
formatter/numpy/globals.py                 1.01     93.7±4.85µs    31.5 MB/sec     1.00    92.7±10.35µs    31.8 MB/sec
formatter/pydantic/types.py                1.07  1899.2±134.52µs    13.4 MB/sec    1.00  1780.7±110.44µs    14.3 MB/sec
linter/all-rules/large/dataset.py          1.05     15.5±0.64ms     2.6 MB/sec     1.00     14.7±0.76ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.10      4.4±0.28ms     3.8 MB/sec     1.00      4.0±0.19ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.07   534.2±41.90µs     5.5 MB/sec     1.00   499.9±27.60µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.9±0.27ms     3.2 MB/sec     1.00      7.7±0.36ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.33ms     4.8 MB/sec     1.01      8.4±0.49ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1806.4±68.64µs     9.2 MB/sec     1.01  1828.7±82.64µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.04   216.0±10.91µs    13.7 MB/sec     1.00   207.5±12.23µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.06      3.9±0.18ms     6.6 MB/sec     1.00      3.6±0.18ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @charliermarsh on 2023-08-16 23:45_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-16 23:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_named_expr.rs`:39 on 2023-08-17 06:13_

Nit: Can we move out the common logic. It makes it easier to reason about what changes based on whether there are comments present or not.
```suggestion

		write!(f, [
			group(&format_args!(target.format(), soft_line_break_or_space())),
          	text(":="),
        ])?;
                
        if dangling.is_empty() {
        	space().fmt(f)?;
        } else {
            write!(
                f,
                [
                    dangling_comments(dangling),
                    hard_line_break(),
                ]
            )?;
        }
        
        value.format().fmt(f)
```

---

_@MichaReiser approved on 2023-08-17 06:17_

We may need to make a decision on how we want to format comments on operators to ensure consistency, see https://github.com/astral-sh/ruff/issues/6062

---

_Merged by @charliermarsh on 2023-08-18 03:10_

---

_Closed by @charliermarsh on 2023-08-18 03:10_

---

_Branch deleted on 2023-08-18 03:10_

---
