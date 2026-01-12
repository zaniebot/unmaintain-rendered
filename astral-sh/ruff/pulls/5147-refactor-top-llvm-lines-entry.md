```yaml
number: 5147
title: Refactor top llvm-lines entry
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: recude_RuleCodePrefix_iter
created_at: 2023-06-16T14:02:50Z
updated_at: 2023-06-18T10:39:07Z
url: https://github.com/astral-sh/ruff/pull/5147
synced_at: 2026-01-12T03:43:30Z
```

# Refactor top llvm-lines entry

---

_Pull request opened by @konstin on 2023-06-16 14:02_

## Summary

This refactors the top entry in terms of llvm lines, `RuleCodePrefix::iter()`. It's only used for generating the schema and the clap completion so no effect on performance. 

I've confirmed with
```
CARGO_TARGET_DIR=target-llvm-lines RUSTFLAGS="-Csymbol-mangling-version=v0" cargo llvm-lines -p ruff --lib | head -n 20
```
that this indeed remove the method from the list of heaviest symbols in terms of llvm-lines

Before:
```
  Lines                  Copies               Function name
  -----                  ------               -------------
  1768469                40538                (TOTAL)
    10391 (0.6%,  0.6%)      1 (0.0%,  0.0%)  <ruff[fa0f2e8ef07114da]::codes::RuleCodePrefix>::iter
     8250 (0.5%,  1.1%)      1 (0.0%,  0.0%)  <ruff[fa0f2e8ef07114da]::codes::Rule>::noqa_code
     7427 (0.4%,  1.5%)      1 (0.0%,  0.0%)  <ruff[fa0f2e8ef07114da]::checkers::ast::Checker as ruff_python_ast[c4c9eadfa5741dd4]::visitor::Visitor>::visit_stmt
     6536 (0.4%,  1.8%)      1 (0.0%,  0.0%)  <<ruff[fa0f2e8ef07114da]::settings::options::Options as serde[1a28808d63625aed]::de::Deserialize>::deserialize::__Visitor as serde[1a28808d63625aed]::de::Visitor>::visit_map::<toml_edit[de4ca26332d39787]::de::spanned::SpannedDeserializer<toml_edit[de4ca26332d39787]::de::value::ValueDeserializer>>
     6536 (0.4%,  2.2%)      1 (0.0%,  0.0%)  <<ruff[fa0f2e8ef07114da]::settings::options::Options as serde[1a28808d63625aed]::de::Deserialize>::deserialize::__Visitor as serde[1a28808d63625aed]::de::Visitor>::visit_map::<toml_edit[de4ca26332d39787]::de::table::TableMapAccess>
     6533 (0.4%,  2.6%)      1 (0.0%,  0.0%)  <<ruff[fa0f2e8ef07114da]::settings::options::Options as serde[1a28808d63625aed]::de::Deserialize>::deserialize::__Visitor as serde[1a28808d63625aed]::de::Visitor>::visit_map::<toml_edit[de4ca26332d39787]::de::datetime::DatetimeDeserializer>
     5727 (0.3%,  2.9%)      1 (0.0%,  0.0%)  <ruff[fa0f2e8ef07114da]::checkers::ast::Checker as ruff_python_ast[c4c9eadfa5741dd4]::visitor::Visitor>::visit_expr
     4453 (0.3%,  3.2%)      1 (0.0%,  0.0%)  ruff[fa0f2e8ef07114da]::flake8_to_ruff::converter::convert
     3790 (0.2%,  3.4%)      1 (0.0%,  0.0%)  <&ruff[fa0f2e8ef07114da]::registry::Linter as core[da82827a87f140f9]::iter::traits::collect::IntoIterator>::into_iter
     3416 (0.2%,  3.6%)      1 (0.0%,  0.0%)  <ruff[fa0f2e8ef07114da]::registry::Linter>::code_for_rule
     3187 (0.2%,  3.7%)      1 (0.0%,  0.0%)  <ruff[fa0f2e8ef07114da]::codes::Rule as core[da82827a87f140f9]::fmt::Debug>::fmt
     3185 (0.2%,  3.9%)      1 (0.0%,  0.0%)  <&str as core[da82827a87f140f9]::convert::From<&ruff[fa0f2e8ef07114da]::codes::Rule>>::from
     3185 (0.2%,  4.1%)      1 (0.0%,  0.0%)  <&str as core[da82827a87f140f9]::convert::From<ruff[fa0f2e8ef07114da]::codes::Rule>>::from
     3185 (0.2%,  4.3%)      1 (0.0%,  0.0%)  <ruff[fa0f2e8ef07114da]::codes::Rule as core[da82827a87f140f9]::convert::AsRef<str>>::as_ref
     3183 (0.2%,  4.5%)      1 (0.0%,  0.0%)  <ruff[fa0f2e8ef07114da]::codes::RuleIter>::get
     2718 (0.2%,  4.6%)      1 (0.0%,  0.0%)  <<ruff[fa0f2e8ef07114da]::settings::options::Options as serde[1a28808d63625aed]::de::Deserialize>::deserialize::__Visitor as serde[1a28808d63625aed]::de::Visitor>::visit_seq::<toml_edit[de4ca26332d39787]::de::array::ArraySeqAccess>
     2706 (0.2%,  4.8%)      1 (0.0%,  0.0%)  <&ruff[fa0f2e8ef07114da]::codes::Pylint as core[da82827a87f140f9]::iter::traits::collect::IntoIterator>::into_iter
```
After:
```
  Lines                  Copies               Function name
  -----                  ------               -------------
  1763380                40806                (TOTAL)
     8250 (0.5%,  0.5%)      1 (0.0%,  0.0%)  <ruff[fa0f2e8ef07114da]::codes::Rule>::noqa_code
     7427 (0.4%,  0.9%)      1 (0.0%,  0.0%)  <ruff[fa0f2e8ef07114da]::checkers::ast::Checker as ruff_python_ast[c4c9eadfa5741dd4]::visitor::Visitor>::visit_stmt
     6536 (0.4%,  1.3%)      1 (0.0%,  0.0%)  <<ruff[fa0f2e8ef07114da]::settings::options::Options as serde[1a28808d63625aed]::de::Deserialize>::deserialize::__Visitor as serde[1a28808d63625aed]::de::Visitor>::visit_map::<toml_edit[de4ca26332d39787]::de::spanned::SpannedDeserializer<toml_edit[de4ca26332d39787]::de::value::ValueDeserializer>>
     6536 (0.4%,  1.6%)      1 (0.0%,  0.0%)  <<ruff[fa0f2e8ef07114da]::settings::options::Options as serde[1a28808d63625aed]::de::Deserialize>::deserialize::__Visitor as serde[1a28808d63625aed]::de::Visitor>::visit_map::<toml_edit[de4ca26332d39787]::de::table::TableMapAccess>
     6533 (0.4%,  2.0%)      1 (0.0%,  0.0%)  <<ruff[fa0f2e8ef07114da]::settings::options::Options as serde[1a28808d63625aed]::de::Deserialize>::deserialize::__Visitor as serde[1a28808d63625aed]::de::Visitor>::visit_map::<toml_edit[de4ca26332d39787]::de::datetime::DatetimeDeserializer>
     5727 (0.3%,  2.3%)      1 (0.0%,  0.0%)  <ruff[fa0f2e8ef07114da]::checkers::ast::Checker as ruff_python_ast[c4c9eadfa5741dd4]::visitor::Visitor>::visit_expr
     4453 (0.3%,  2.6%)      1 (0.0%,  0.0%)  ruff[fa0f2e8ef07114da]::flake8_to_ruff::converter::convert
     3790 (0.2%,  2.8%)      1 (0.0%,  0.0%)  <&ruff[fa0f2e8ef07114da]::registry::Linter as core[da82827a87f140f9]::iter::traits::collect::IntoIterator>::into_iter
     3416 (0.2%,  3.0%)      1 (0.0%,  0.0%)  <ruff[fa0f2e8ef07114da]::registry::Linter>::code_for_rule
     3187 (0.2%,  3.2%)      1 (0.0%,  0.0%)  <ruff[fa0f2e8ef07114da]::codes::Rule as core[da82827a87f140f9]::fmt::Debug>::fmt
     3185 (0.2%,  3.3%)      1 (0.0%,  0.0%)  <&str as core[da82827a87f140f9]::convert::From<&ruff[fa0f2e8ef07114da]::codes::Rule>>::from
     3185 (0.2%,  3.5%)      1 (0.0%,  0.0%)  <&str as core[da82827a87f140f9]::convert::From<ruff[fa0f2e8ef07114da]::codes::Rule>>::from
     3185 (0.2%,  3.7%)      1 (0.0%,  0.0%)  <ruff[fa0f2e8ef07114da]::codes::Rule as core[da82827a87f140f9]::convert::AsRef<str>>::as_ref
     3183 (0.2%,  3.9%)      1 (0.0%,  0.0%)  <ruff[fa0f2e8ef07114da]::codes::RuleIter>::get
     2718 (0.2%,  4.0%)      1 (0.0%,  0.0%)  <<ruff[fa0f2e8ef07114da]::settings::options::Options as serde[1a28808d63625aed]::de::Deserialize>::deserialize::__Visitor as serde[1a28808d63625aed]::de::Visitor>::visit_seq::<toml_edit[de4ca26332d39787]::de::array::ArraySeqAccess>
     2706 (0.2%,  4.2%)      1 (0.0%,  0.0%)  <&ruff[fa0f2e8ef07114da]::codes::Pylint as core[da82827a87f140f9]::iter::traits::collect::IntoIterator>::into_iter
     2573 (0.1%,  4.3%)      1 (0.0%,  0.0%)  <<ruff[fa0f2e8ef07114da]::rules::isort::settings::Options as serde[1a28808d63625aed]::de::Deserialize>::deserialize::__Visitor as serde[1a28808d63625aed]::de::Visitor>::visit_map::<toml_edit[de4ca26332d39787]::de::spanned::SpannedDeserializer<toml_edit[de4ca26332d39787]::de::value::ValueDeserializer>>
```
I didn't measure the effect on binary size this time.

## Testing

`cargo test` which uses this to generate the schema didn't change

---

_Label `internal` added by @konstin on 2023-06-16 14:05_

---

_Comment by @charliermarsh on 2023-06-16 14:13_

Wow, `noqa_code` too!

---

_Comment by @github-actions[bot] on 2023-06-16 14:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05      8.9±0.38ms     4.6 MB/sec    1.00      8.5±0.34ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.06  1837.7±69.01µs     9.1 MB/sec    1.00  1736.3±74.72µs     9.6 MB/sec
formatter/numpy/globals.py                 1.06   180.0±20.29µs    16.4 MB/sec    1.00   169.5±14.64µs    17.4 MB/sec
formatter/pydantic/types.py                1.05      3.6±0.21ms     7.0 MB/sec    1.00      3.5±0.18ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.04     17.6±0.39ms     2.3 MB/sec    1.00     17.0±0.68ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.16ms     4.0 MB/sec    1.00      4.2±0.16ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.09   560.9±26.48µs     5.3 MB/sec    1.00   514.9±38.77µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.5±0.23ms     3.4 MB/sec    1.00      7.4±0.28ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.06      9.2±0.33ms     4.4 MB/sec    1.00      8.6±0.37ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1963.3±75.95µs     8.5 MB/sec    1.00  1931.7±60.80µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    230.9±9.40µs    12.8 MB/sec    1.00   227.7±10.05µs    13.0 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.2±0.23ms     6.1 MB/sec    1.00      4.0±0.18ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.0±0.24ms     4.5 MB/sec    1.19     10.8±0.35ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1830.3±68.91µs     9.1 MB/sec    1.21      2.2±0.08ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00   189.9±12.23µs    15.5 MB/sec    1.16   219.6±17.18µs    13.4 MB/sec
formatter/pydantic/types.py                1.00      3.7±0.13ms     6.9 MB/sec    1.17      4.4±0.18ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.01     19.3±0.63ms     2.1 MB/sec    1.00     19.2±0.68ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.0±0.26ms     3.4 MB/sec    1.00      4.9±0.16ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   571.1±17.74µs     5.2 MB/sec    1.06   604.5±40.01µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.5±0.29ms     3.0 MB/sec    1.00      8.4±0.47ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.01      9.7±0.24ms     4.2 MB/sec    1.00      9.7±0.27ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.0±0.07ms     8.2 MB/sec    1.00      2.0±0.07ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.01   244.6±22.18µs    12.1 MB/sec    1.00    243.2±8.78µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.13ms     5.8 MB/sec    1.00      4.4±0.15ms     5.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @konstin on 2023-06-16 14:17_

tbh i'd leave noqa_code intact, 10k llvm lines seems like good cutoff for optimizing, afterwards it becomes low prio

---

_Review comment by @charliermarsh on `crates/ruff_macros/src/map_codes.rs`:358 on 2023-06-16 15:12_

Why do we need the `std::iter::empty` and chain?

---

_@charliermarsh approved on 2023-06-16 15:12_

---

_@konstin reviewed on 2023-06-16 15:18_

---

_Review comment by @konstin on `crates/ruff_macros/src/map_codes.rs`:358 on 2023-06-16 15:18_

Putting every linter in a slice generates a type error (the map contains the type of the linter group); I used `iter::empty()` because afaik quote has no utils for special casing the first entry entry in an iteration.

---

_Merged by @konstin on 2023-06-18 10:39_

---

_Closed by @konstin on 2023-06-18 10:39_

---

_Branch deleted on 2023-06-18 10:39_

---
