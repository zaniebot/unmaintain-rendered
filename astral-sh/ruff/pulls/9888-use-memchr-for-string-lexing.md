```yaml
number: 9888
title: "Use `memchr` for string lexing"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/l
created_at: 2024-02-08T03:53:39Z
updated_at: 2024-02-08T17:36:23Z
url: https://github.com/astral-sh/ruff/pull/9888
synced_at: 2026-01-10T22:57:09Z
```

# Use `memchr` for string lexing

---

_Pull request opened by @charliermarsh on 2024-02-08 03:53_

## Summary

On `main`, string lexing consists of walking through the string character-by-character to search for the closing quote (with some nuance: we also need to skip escaped characters, and error if we see newlines in non-triple-quoted strings). This PR rewrites `lex_string` to instead use `memchr` to search for the closing quote, which is significantly faster. On my machine, at least, the `globals.py` benchmark (which contains a lot of docstrings) gets 40% faster...

```text
lexer/numpy/globals.py  time:   [3.6410 µs 3.6496 µs 3.6585 µs]
                        thrpt:  [806.53 MiB/s 808.49 MiB/s 810.41 MiB/s]
                 change:
                        time:   [-40.413% -40.185% -39.984%] (p = 0.00 < 0.05)
                        thrpt:  [+66.623% +67.181% +67.822%]
                        Performance has improved.
Found 2 outliers among 100 measurements (2.00%)
  2 (2.00%) high mild
lexer/unicode/pypinyin.py
                        time:   [12.422 µs 12.445 µs 12.467 µs]
                        thrpt:  [337.03 MiB/s 337.65 MiB/s 338.27 MiB/s]
                 change:
                        time:   [-9.4213% -9.1930% -8.9586%] (p = 0.00 < 0.05)
                        thrpt:  [+9.8401% +10.124% +10.401%]
                        Performance has improved.
Found 3 outliers among 100 measurements (3.00%)
  1 (1.00%) high mild
  2 (2.00%) high severe
lexer/pydantic/types.py time:   [107.45 µs 107.50 µs 107.56 µs]
                        thrpt:  [237.11 MiB/s 237.24 MiB/s 237.35 MiB/s]
                 change:
                        time:   [-4.0108% -3.7005% -3.3787%] (p = 0.00 < 0.05)
                        thrpt:  [+3.4968% +3.8427% +4.1784%]
                        Performance has improved.
Found 7 outliers among 100 measurements (7.00%)
  2 (2.00%) high mild
  5 (5.00%) high severe
lexer/numpy/ctypeslib.py
                        time:   [46.123 µs 46.165 µs 46.208 µs]
                        thrpt:  [360.36 MiB/s 360.69 MiB/s 361.01 MiB/s]
                 change:
                        time:   [-19.313% -18.996% -18.710%] (p = 0.00 < 0.05)
                        thrpt:  [+23.016% +23.451% +23.935%]
                        Performance has improved.
Found 8 outliers among 100 measurements (8.00%)
  3 (3.00%) low mild
  1 (1.00%) high mild
  4 (4.00%) high severe
lexer/large/dataset.py  time:   [231.07 µs 231.19 µs 231.33 µs]
                        thrpt:  [175.87 MiB/s 175.97 MiB/s 176.06 MiB/s]
                 change:
                        time:   [-2.0437% -1.7663% -1.4922%] (p = 0.00 < 0.05)
                        thrpt:  [+1.5148% +1.7981% +2.0864%]
                        Performance has improved.
Found 10 outliers among 100 measurements (10.00%)
  5 (5.00%) high mild
  5 (5.00%) high severe
```


---

_Label `performance` added by @charliermarsh on 2024-02-08 03:53_

---

_Comment by @codspeed-hq[bot] on 2024-02-08 04:00_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/l)

### Merging #9888 will **improve performances by 40.35%**

<sub>Comparing <code>charlie/l</code> (4b4b298) with <code>main</code> (ad313b9)</sub>



### Summary

`⚡ 4` improvements
`✅ 26` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/l` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `lexer[unicode/pypinyin.py]` | 571.5 µs | 543.4 µs | +5.18% |
| ⚡ | `lexer[numpy/ctypeslib.py]` | 1.8 ms | 1.7 ms | +11.64% |
| ⚡ | `lexer[numpy/globals.py]` | 223.5 µs | 159.3 µs | +40.35% |
| ⚡ | `parser[numpy/globals.py]` | 1.1 ms | 1.1 ms | +6.07% |


---

_Review requested from @BurntSushi by @charliermarsh on 2024-02-08 04:20_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-02-08 04:20_

---

_Comment by @github-actions[bot] on 2024-02-08 04:37_

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

_@MichaReiser reviewed on 2024-02-08 11:39_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:697 on 2024-02-08 11:39_

Nit: I find the `let Some(...) .. else` use here slightly confusing because the `else` block is so long. That's why I would prefer a regular `if let Some(index) {...} else {...}` here

---

_@MichaReiser approved on 2024-02-08 11:44_

Nice!

---

_@MichaReiser approved on 2024-02-08 11:44_

Nice!

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/src/lexer.rs`:697 on 2024-02-08 14:43_

I don't think I mind the `let ... else`, but I'd probably do `u8::try_from(quote).expect("char that fits in u8")` here instead of `quote as u8`.

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/src/lexer.rs`:749 on 2024-02-08 14:45_

Same as above for `quote` (or just do `let quote_byte = u8::try_from(quote).unwrap()` at the top of the function once). But for `'\r' as u8` and `'\n' as u8`, you can just write `b'\r'` and `b'\n'`, respectively.

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/src/lexer.rs`:746 on 2024-02-08 14:45_

Ah I see now why you were asking about `memchr3`. :P 

---

_@BurntSushi approved on 2024-02-08 14:46_

Nice, I like it!

---

_Merged by @charliermarsh on 2024-02-08 17:23_

---

_Closed by @charliermarsh on 2024-02-08 17:23_

---

_Branch deleted on 2024-02-08 17:23_

---
