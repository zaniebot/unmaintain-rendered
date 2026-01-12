```yaml
number: 9885
title: "Reduce `Result<Tok, LexicalError>` size by using `Box<str>` instead of `String`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
assignees: []
merged: true
base: main
head: reduce-tok-size
created_at: 2024-02-07T22:16:18Z
updated_at: 2024-02-08T20:36:23Z
url: https://github.com/astral-sh/ruff/pull/9885
synced_at: 2026-01-12T15:55:30Z
```

# Reduce `Result<Tok, LexicalError>` size by using `Box<str>` instead of `String`

---

_@MichaReiser_

* Reduce the `LexicalError` type by using `Box<str>` over `String`
* Reduce the `Tok` size by using `Box<str>` over `String`. 

Using `Box<str>` is sufficient for us because we don't want to mutate the error message or token value.

The downside of this is that using `Box<str>` is slightly more annoying.

Future: It would be interesting to change the Lexer signature always to return a `Tok::Invalid` and write the errors to a `Vec` instead. This should allow for drastically reducing the size of `Tok` and `Spanned.` 

---

_Comment by @codspeed-hq[bot] on 2024-02-07 22:23_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/reduce-tok-size)

### Merging #9885 will **not alter performance**

<sub>Comparing <code>reduce-tok-size</code> (7a7f01c) with <code>main</code> (9027169)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-02-07 23:31_

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

_Label `performance` added by @MichaReiser on 2024-02-08 03:23_

---

_Review requested from @BurntSushi by @MichaReiser on 2024-02-08 03:24_

---

_Renamed from "Reduce `Tok` size by using `Box<str>` instead of `String`" to "Reduce `Tok` and `Result<Tok, LexicalError>` sizes by using `Box<str>` instead of `String`" by @MichaReiser on 2024-02-08 12:03_

---

_Renamed from "Reduce `Tok` and `Result<Tok, LexicalError>` sizes by using `Box<str>` instead of `String`" to "Reduce `Result<Tok, LexicalError>` size by using `Box<str>` instead of `String`" by @MichaReiser on 2024-02-08 12:03_

---

_Closed by @MichaReiser on 2024-02-08 12:04_

---

_Reopened by @MichaReiser on 2024-02-08 12:04_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:77 on 2024-02-08 14:49_

I don't think you want `.as_ref()` here. Generally one should only use `as_ref()` when a `AsRef` bound is in scope, otherwise you're susceptible to inference failures in future Rust releases.

If `value` is a `Box<str>`, then you can get a `&str` with `&*value`. If `value` is a `&Box<str>` (which is I think the case here), then you can get a `&str` with `&**value`.

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:77 on 2024-02-08 14:49_

And I would generally apply this same comment to I believe all other uses of `as_ref()` in this PR.

---

_@BurntSushi reviewed on 2024-02-08 14:51_

Nice!!!

---

_@BurntSushi reviewed on 2024-02-08 14:57_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:77 on 2024-02-08 14:57_

Some references:

* https://github.com/rust-lang/rust/issues/60958
* https://github.com/rust-lang/rust/issues/62586

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:77 on 2024-02-08 14:59_

Makes sense. I like `as_ref` because I don't need to guess the number of `*` that I have to use 😆 but I can change it. 

I'm somewhat hesitant of merging the PR because I don't see an improvement when running the lexer benchmarks locally :(

---

_@MichaReiser reviewed on 2024-02-08 14:59_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:77 on 2024-02-08 17:01_

I do see a very small improvement on my machine:

```
$ critcmp base test
group                        base                                   test
-----                        ----                                   ----
lexer/large/dataset.py       1.01    202.0±0.66µs   201.4 MB/sec    1.00    200.9±0.51µs   202.5 MB/sec
lexer/numpy/ctypeslib.py     1.03     48.9±0.15µs   340.5 MB/sec    1.00     47.7±0.17µs   349.2 MB/sec
lexer/numpy/globals.py       1.04      5.0±0.02µs   592.9 MB/sec    1.00      4.8±0.01µs   616.2 MB/sec
lexer/pydantic/types.py      1.02     93.8±0.25µs   271.9 MB/sec    1.00     92.2±0.37µs   276.6 MB/sec
lexer/unicode/pypinyin.py    1.01     12.5±0.03µs   335.4 MB/sec    1.00     12.4±0.05µs   337.6 MB/sec
```

I'd say ship it!

---

_@BurntSushi reviewed on 2024-02-08 17:02_

---

_Comment by @charliermarsh on 2024-02-08 17:09_

I'm not kidding I was thinking about this as I fell asleep a few nights ago.

---

_Marked ready for review by @MichaReiser on 2024-02-08 18:54_

---

_Merged by @MichaReiser on 2024-02-08 20:36_

---

_Closed by @MichaReiser on 2024-02-08 20:36_

---

_Branch deleted on 2024-02-08 20:36_

---
