```yaml
number: 4926
title: Use phf for confusables to reduce llvm lines
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: phf-for-confusable
created_at: 2023-06-07T12:33:48Z
updated_at: 2023-06-08T06:13:22Z
url: https://github.com/astral-sh/ruff/pull/4926
synced_at: 2026-01-12T03:43:29Z
```

# Use phf for confusables to reduce llvm lines

---

_Pull request opened by @konstin on 2023-06-07 12:33_

## Summary

This replaces FxHashMap for the confusables with a perfect hash map from the [phf crate](https://github.com/rust-phf/rust-phf) to reduce the generated llvm instructions.

A perfect hash function is one that doesn't have any collisions. We can build one because we know all keys at compile time. This improves hashmap efficiency, even though this is likely not noticeable in our case (except someone has a large non-english crate to test on).

The original hashmap contained a lot of duplicates, which i had to remove when phf_map complained, i did so by sorting the keys.

The important part that it reduces the llvm instructions generated (#3808, `RUSTFLAGS="-Csymbol-mangling-version=v0" cargo llvm-lines -p ruff --lib | head -20`):

```
  Lines                  Copies               Function name
  -----                  ------               -------------
  1740502                38973                (TOTAL)
    27423 (1.6%,  1.6%)      1 (0.0%,  0.0%)  ruff[cef4c65d96248843]::rules::ruff::rules::confusables::CONFUSABLES::{closure#0}
    10193 (0.6%,  2.2%)      1 (0.0%,  0.0%)  <ruff[cef4c65d96248843]::codes::RuleCodePrefix>::iter
     8107 (0.5%,  2.6%)      1 (0.0%,  0.0%)  <ruff[cef4c65d96248843]::codes::Rule>::noqa_code
     7345 (0.4%,  3.0%)      1 (0.0%,  0.0%)  <ruff[cef4c65d96248843]::checkers::ast::Checker as ruff_python_ast[3778b140caf21545]::visitor::Visitor>::visit_stmt
     6412 (0.4%,  3.4%)      1 (0.0%,  0.0%)  <<ruff[cef4c65d96248843]::settings::options::Options as serde[d89b1b632568f5a3]::de::Deserialize>::deserialize::__Visitor as serde[d89b1b632568f5a3]::de::Visitor>::visit_map::<toml_edit[7e3a6c5e67260672]::de::spanned::SpannedDeserializer<toml_edit[7e3a6c5e67260672]::de::value::ValueDeserializer>>
     6412 (0.4%,  3.8%)      1 (0.0%,  0.0%)  <<ruff[cef4c65d96248843]::settings::options::Options as serde[d89b1b632568f5a3]::de::Deserialize>::deserialize::__Visitor as serde[d89b1b632568f5a3]::de::Visitor>::visit_map::<toml_edit[7e3a6c5e67260672]::de::table::TableMapAccess>
     6409 (0.4%,  4.2%)      1 (0.0%,  0.0%)  <<ruff[cef4c65d96248843]::settings::options::Options as serde[d89b1b632568f5a3]::de::Deserialize>::deserialize::__Visitor as serde[d89b1b632568f5a3]::de::Visitor>::visit_map::<toml_edit[7e3a6c5e67260672]::de::datetime::DatetimeDeserializer>
     5696 (0.3%,  4.5%)      1 (0.0%,  0.0%)  <ruff[cef4c65d96248843]::checkers::ast::Checker as ruff_python_ast[3778b140caf21545]::visitor::Visitor>::visit_expr
     4448 (0.3%,  4.7%)      1 (0.0%,  0.0%)  ruff[cef4c65d96248843]::flake8_to_ruff::converter::convert
     3702 (0.2%,  4.9%)      1 (0.0%,  0.0%)  <&ruff[cef4c65d96248843]::registry::Linter as core[da82827a87f140f9]::iter::traits::collect::IntoIterator>::into_iter
     3349 (0.2%,  5.1%)      1 (0.0%,  0.0%)  <ruff[cef4c65d96248843]::registry::Linter>::code_for_rule
     3132 (0.2%,  5.3%)      1 (0.0%,  0.0%)  <ruff[cef4c65d96248843]::codes::Rule as core[da82827a87f140f9]::fmt::Debug>::fmt
     3130 (0.2%,  5.5%)      1 (0.0%,  0.0%)  <&str as core[da82827a87f140f9]::convert::From<&ruff[cef4c65d96248843]::codes::Rule>>::from
     3130 (0.2%,  5.7%)      1 (0.0%,  0.0%)  <&str as core[da82827a87f140f9]::convert::From<ruff[cef4c65d96248843]::codes::Rule>>::from
     3130 (0.2%,  5.9%)      1 (0.0%,  0.0%)  <ruff[cef4c65d96248843]::codes::Rule as core[da82827a87f140f9]::convert::AsRef<str>>::as_ref
     3128 (0.2%,  6.0%)      1 (0.0%,  0.0%)  <ruff[cef4c65d96248843]::codes::RuleIter>::get
     2669 (0.2%,  6.2%)      1 (0.0%,  0.0%)  <<ruff[cef4c65d96248843]::settings::options::Options as serde[d89b1b632568f5a3]::de::Deserialize>::deserialize::__Visitor as serde[d89b1b632568f5a3]::de::Visitor>::visit_seq::<toml_edit[7e3a6c5e67260672]::de::array::ArraySeqAccess>
```
After:
```
  Lines                  Copies               Function name
  -----                  ------               -------------
  1710487                38900                (TOTAL)
    10193 (0.6%,  0.6%)      1 (0.0%,  0.0%)  <ruff[52408f46d2058296]::codes::RuleCodePrefix>::iter
     8107 (0.5%,  1.1%)      1 (0.0%,  0.0%)  <ruff[52408f46d2058296]::codes::Rule>::noqa_code
     7345 (0.4%,  1.5%)      1 (0.0%,  0.0%)  <ruff[52408f46d2058296]::checkers::ast::Checker as ruff_python_ast[5588cd60041c8605]::visitor::Visitor>::visit_stmt
     6412 (0.4%,  1.9%)      1 (0.0%,  0.0%)  <<ruff[52408f46d2058296]::settings::options::Options as serde[d89b1b632568f5a3]::de::Deserialize>::deserialize::__Visitor as serde[d89b1b632568f5a3]::de::Visitor>::visit_map::<toml_edit[7e3a6c5e67260672]::de::spanned::SpannedDeserializer<toml_edit[7e3a6c5e67260672]::de::value::ValueDeserializer>>
     6412 (0.4%,  2.2%)      1 (0.0%,  0.0%)  <<ruff[52408f46d2058296]::settings::options::Options as serde[d89b1b632568f5a3]::de::Deserialize>::deserialize::__Visitor as serde[d89b1b632568f5a3]::de::Visitor>::visit_map::<toml_edit[7e3a6c5e67260672]::de::table::TableMapAccess>
     6409 (0.4%,  2.6%)      1 (0.0%,  0.0%)  <<ruff[52408f46d2058296]::settings::options::Options as serde[d89b1b632568f5a3]::de::Deserialize>::deserialize::__Visitor as serde[d89b1b632568f5a3]::de::Visitor>::visit_map::<toml_edit[7e3a6c5e67260672]::de::datetime::DatetimeDeserializer>
     5696 (0.3%,  3.0%)      1 (0.0%,  0.0%)  <ruff[52408f46d2058296]::checkers::ast::Checker as ruff_python_ast[5588cd60041c8605]::visitor::Visitor>::visit_expr
     4448 (0.3%,  3.2%)      1 (0.0%,  0.0%)  ruff[52408f46d2058296]::flake8_to_ruff::converter::convert
     3702 (0.2%,  3.4%)      1 (0.0%,  0.0%)  <&ruff[52408f46d2058296]::registry::Linter as core[da82827a87f140f9]::iter::traits::collect::IntoIterator>::into_iter
     3349 (0.2%,  3.6%)      1 (0.0%,  0.0%)  <ruff[52408f46d2058296]::registry::Linter>::code_for_rule
     3132 (0.2%,  3.8%)      1 (0.0%,  0.0%)  <ruff[52408f46d2058296]::codes::Rule as core[da82827a87f140f9]::fmt::Debug>::fmt
     3130 (0.2%,  4.0%)      1 (0.0%,  0.0%)  <&str as core[da82827a87f140f9]::convert::From<&ruff[52408f46d2058296]::codes::Rule>>::from
     3130 (0.2%,  4.2%)      1 (0.0%,  0.0%)  <&str as core[da82827a87f140f9]::convert::From<ruff[52408f46d2058296]::codes::Rule>>::from
     3130 (0.2%,  4.4%)      1 (0.0%,  0.0%)  <ruff[52408f46d2058296]::codes::Rule as core[da82827a87f140f9]::convert::AsRef<str>>::as_ref
     3128 (0.2%,  4.5%)      1 (0.0%,  0.0%)  <ruff[52408f46d2058296]::codes::RuleIter>::get
     2669 (0.2%,  4.7%)      1 (0.0%,  0.0%)  <<ruff[52408f46d2058296]::settings::options::Options as serde[d89b1b632568f5a3]::de::Deserialize>::deserialize::__Visitor as serde[d89b1b632568f5a3]::de::Visitor>::visit_seq::<toml_edit[7e3a6c5e67260672]::de::array::ArraySeqAccess>
     2659 (0.2%,  4.9%)      1 (0.0%,  0.0%)  <&ruff[52408f46d2058296]::codes::Pylint as core[da82827a87f140f9]::iter::traits::collect::IntoIterator>::into_iter
```

I'd assume this has a positive effect both on compile time and on runtime, but i don't know the actual effect on compile times and can't really measure.

## Test plan

Check CI for any performance regressions.

This should fix #3808 if we merge it.


---

_Comment by @konstin on 2023-06-07 12:34_

Current dependencies on/for this PR:
* main
  * **PR #4926** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/4926" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/charliermarsh/ruff/4926?utm_source=stack-comment).

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/confusables.rs`:1 on 2023-06-07 12:48_

This file is autogenerated, so I think we need to update the generation script instead.

---

_@charliermarsh reviewed on 2023-06-07 12:48_

---

_Comment by @charliermarsh on 2023-06-07 12:48_

Any way we can benchmark the effect on runtime?

---

_Comment by @github-actions[bot] on 2023-06-07 12:49_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.5±0.02ms     7.4 MB/sec    1.08      5.9±0.03ms     6.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1087.1±3.82µs    15.3 MB/sec    1.14   1235.6±1.60µs    13.5 MB/sec
formatter/numpy/globals.py                 1.00    120.6±0.15µs    24.5 MB/sec    1.17    141.1±0.14µs    20.9 MB/sec
formatter/pydantic/types.py                1.00      2.4±0.01ms    10.6 MB/sec    1.09      2.6±0.02ms     9.7 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.09ms     2.9 MB/sec    1.00     14.1±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     5.0 MB/sec    1.01      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    410.9±0.54µs     7.2 MB/sec    1.02    417.9±0.41µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.9±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.0 MB/sec    1.01      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1452.0±3.96µs    11.5 MB/sec    1.02   1482.3±3.56µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.4±0.21µs    18.5 MB/sec    1.03    164.7±0.32µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.01      3.1±0.01ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.17      9.9±0.63ms     4.1 MB/sec    1.00      8.4±0.22ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.06  1823.8±93.03µs     9.1 MB/sec    1.00  1718.2±50.38µs     9.7 MB/sec
formatter/numpy/globals.py                 1.01   204.0±17.18µs    14.5 MB/sec    1.00   201.8±10.87µs    14.6 MB/sec
formatter/pydantic/types.py                1.14      4.2±0.33ms     6.0 MB/sec    1.00      3.7±0.14ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.00     21.5±0.69ms  1938.1 KB/sec    1.01     21.7±0.64ms  1924.1 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.15ms     3.2 MB/sec    1.03      5.4±0.19ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   608.9±22.10µs     4.8 MB/sec    1.02   622.2±18.80µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.22ms     3.0 MB/sec    1.04      9.0±0.28ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.02     10.3±0.34ms     4.0 MB/sec    1.00     10.0±0.25ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.09      2.3±0.10ms     7.3 MB/sec    1.00      2.1±0.06ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.16   286.9±23.36µs    10.3 MB/sec    1.00    248.1±7.93µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.15      5.2±0.48ms     4.9 MB/sec    1.00      4.6±0.12ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @konstin on 2023-06-07 12:50_

> Any way we can benchmark the effect on runtime?

The CI perf checks should pop up soon. I don't have experience with the phf crate, but a phf is expected to be faster because there are no collision.

---

_Comment by @konstin on 2023-06-07 12:52_

> This file is autogenerated, so I think we need to update the generation script instead.

If we see that this is the right structure, i'll update `update_ambiguous_characters.py` to emit phf generation, if not i'll update it to not emit duplicates anymore

---

_@MichaReiser requested changes on 2023-06-07 12:54_

Nice. Putting back in your queue to update the generation script (if it improves perf). 

You can also use `cargo bloat --bin ruff --release` to measure the estimated size

---

_Comment by @konstin on 2023-06-07 14:20_

byte sizes of the assets (`ls -la`) | before | after | improvement
---|---|---|---
target/release/ruff | 15177064 | 14906664 | -270.4 kB
target/wheels/ruff-0.0.271-py3-none-manylinux_2_34_x86_64.whl | 5801783 | 5729345 | -72.4 kB

I want to use phf for this reason alone


---

_Review requested from @MichaReiser by @konstin on 2023-06-07 15:24_

---

_@MichaReiser approved on 2023-06-07 16:06_

---

_@charliermarsh approved on 2023-06-07 21:06_

---

_Merged by @konstin on 2023-06-08 06:13_

---

_Closed by @konstin on 2023-06-08 06:13_

---

_Branch deleted on 2023-06-08 06:13_

---
