```yaml
number: 13818
title: "Lexer: Avoid `drop` branches when setting the new token value"
type: pull_request
state: closed
author: MichaReiser
labels:
  - performance
  - parser
assignees: []
base: main
head: micha/lexer-perf-drop
created_at: 2024-10-19T11:06:20Z
updated_at: 2024-10-19T18:52:39Z
url: https://github.com/astral-sh/ruff/pull/13818
synced_at: 2026-01-12T15:55:45Z
```

# Lexer: Avoid `drop` branches when setting the new token value

---

_@MichaReiser_

## Summary

The `Lexer` resets the `CurrentTokenValue` on every `next_token` call. 
However, Rust can't prove that `current_value` is always `None` when we repalce the current (`None`) value with the lexed value in the `lex_*` branches. 
That's why Rust still inserts the code for dropping the current value. 

This PR introduces a wrapper using `ManuallyDrop` to make use of the knowledge that the `TokenValue` is guaranteed to always be `None` 
when we set a new value. 

## Test Plan

`cargo test`

These are my local numbers. I ran them multiple times. The baseline is this branch (that's why negative numbers are good)

```
lexer/numpy/globals.py  time:   [3.0014 µs 3.0036 µs 3.0058 µs]
                        thrpt:  [981.66 MiB/s 982.38 MiB/s 983.11 MiB/s]
                 change:
                        time:   [+5.2272% +5.5998% +5.9694%] (p = 0.00 < 0.05)
                        thrpt:  [-5.6331% -5.3028% -4.9676%]
                        Performance has regressed.
Found 8 outliers among 100 measurements (8.00%)
  1 (1.00%) low severe
  5 (5.00%) low mild
  2 (2.00%) high mild
lexer/unicode/pypinyin.py
                        time:   [11.065 µs 11.123 µs 11.184 µs]
                        thrpt:  [375.71 MiB/s 377.75 MiB/s 379.73 MiB/s]
                 change:
                        time:   [+6.4901% +6.7363% +7.0050%] (p = 0.00 < 0.05)
                        thrpt:  [-6.5464% -6.3111% -6.0946%]
                        Performance has regressed.
Found 9 outliers among 100 measurements (9.00%)
  1 (1.00%) low mild
  8 (8.00%) high severe
lexer/pydantic/types.py time:   [84.974 µs 85.076 µs 85.206 µs]
                        thrpt:  [299.31 MiB/s 299.77 MiB/s 300.13 MiB/s]
                 change:
                        time:   [+3.4216% +3.9485% +4.5291%] (p = 0.00 < 0.05)
                        thrpt:  [-4.3329% -3.7985% -3.3084%]
                        Performance has regressed.
lexer/numpy/ctypeslib.py
                        time:   [35.493 µs 35.572 µs 35.677 µs]
                        thrpt:  [466.71 MiB/s 468.09 MiB/s 469.15 MiB/s]
                 change:
                        time:   [+3.7056% +4.1303% +4.5415%] (p = 0.00 < 0.05)
                        thrpt:  [-4.3442% -3.9665% -3.5732%]
                        Performance has regressed.
Found 2 outliers among 100 measurements (2.00%)
  1 (1.00%) high mild
  1 (1.00%) high severe
lexer/large/dataset.py  time:   [196.29 µs 196.39 µs 196.50 µs]
                        thrpt:  [207.04 MiB/s 207.15 MiB/s 207.26 MiB/s]
                 change:
                        time:   [+5.2512% +5.6298% +5.9938%] (p = 0.00 < 0.05)
                        thrpt:  [-5.6548% -5.3298% -4.9892%]
                        Performance has regressed.
Found 3 outliers among 100 measurements (3.00%)
  2 (2.00%) low mild
  1 (1.00%) high severe


```

but maybe that's just noise... Very hard to tell :(

---

_Comment by @MichaReiser on 2024-10-19 11:19_

Hmm, this gives me a 4-10% perf improvement locally

---

_Label `performance` added by @MichaReiser on 2024-10-19 11:35_

---

_Label `parser` added by @MichaReiser on 2024-10-19 11:35_

---

_Marked ready for review by @MichaReiser on 2024-10-19 11:35_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-10-19 11:35_

---

_Comment by @github-actions[bot] on 2024-10-19 11:39_

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

_Comment by @MichaReiser on 2024-10-19 12:05_

Looking at a profile, drop only takes about 0.4% of time (except in the `next_token` function but we can't remove it from there.

---

_Closed by @MichaReiser on 2024-10-19 12:05_

---

_Branch deleted on 2024-10-19 18:52_

---
