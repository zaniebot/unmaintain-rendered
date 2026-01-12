```yaml
number: 6018
title: "[`flake8-pyi`] Implement PYI018"
type: pull_request
state: merged
author: LaBatata101
labels:
  - rule
assignees: []
merged: true
base: main
head: PYI018
created_at: 2023-07-23T21:07:41Z
updated_at: 2023-07-27T21:04:15Z
url: https://github.com/astral-sh/ruff/pull/6018
synced_at: 2026-01-12T15:55:20Z
```

# [`flake8-pyi`] Implement PYI018

---

_@LaBatata101_

## Summary

Check for unused private `TypeVar`.  See [original implementation](https://github.com/PyCQA/flake8-pyi/blob/2a86db8271dc500247a8a69419536240334731cf/pyi.py#L1958).

```
$ flake8 --select Y018 crates/ruff/resources/test/fixtures/flake8_pyi/PYI018.pyi

crates/ruff/resources/test/fixtures/flake8_pyi/PYI018.pyi:4:1: Y018 TypeVar "_T" is not used
crates/ruff/resources/test/fixtures/flake8_pyi/PYI018.pyi:5:1: Y018 TypeVar "_P" is not used
```

```
$ ./target/debug/ruff --select PYI018 crates/ruff/resources/test/fixtures/flake8_pyi/PYI018.pyi --no-cache

crates/ruff/resources/test/fixtures/flake8_pyi/PYI018.pyi:4:1: PYI018 TypeVar `_T` is never used
crates/ruff/resources/test/fixtures/flake8_pyi/PYI018.pyi:5:1: PYI018 TypeVar `_P` is never used
Found 2 errors.
```
In the file `unused_private_type_declaration.rs`, I'm planning to add other rules that are similar to `PYI018` like the `PYI046`, `PYI047` and `PYI049`.

I'm not happy with all this nesting that is happening here:
https://github.com/astral-sh/ruff/blob/a8377770b21706ad82bee7f1230ef5e9d2ddaa12/crates/ruff/src/checkers/ast/mod.rs#L4486-L4502
I couldn't figure it out, a way to eliminate it.

ref #848

## Test Plan

Snapshots and manual runs of flake8.

---

_Comment by @github-actions[bot] on 2023-07-23 21:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.0±0.07ms     4.5 MB/sec    1.13     10.1±0.05ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1715.8±4.85µs     9.7 MB/sec    1.10   1886.2±6.22µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    178.5±1.30µs    16.5 MB/sec    1.07    190.9±0.40µs    15.5 MB/sec
formatter/pydantic/types.py                1.00      3.8±0.02ms     6.8 MB/sec    1.10      4.2±0.01ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.01     12.8±0.17ms     3.2 MB/sec    1.00     12.8±0.14ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.2±0.03ms     5.2 MB/sec    1.00      3.2±0.01ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    341.8±1.26µs     8.6 MB/sec    1.01    344.4±5.27µs     8.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.05ms     4.5 MB/sec    1.00      5.7±0.06ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.01      6.4±0.03ms     6.4 MB/sec    1.00      6.4±0.03ms     6.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1270.1±3.58µs    13.1 MB/sec    1.00   1271.2±4.17µs    13.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    124.5±0.75µs    23.7 MB/sec    1.00    125.0±1.42µs    23.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.01ms     9.2 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06     10.8±0.10ms     3.8 MB/sec    1.00     10.2±0.09ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.05      2.0±0.03ms     8.1 MB/sec    1.00  1950.6±27.82µs     8.5 MB/sec
formatter/numpy/globals.py                 1.02    222.3±5.42µs    13.3 MB/sec    1.00    217.3±7.43µs    13.6 MB/sec
formatter/pydantic/types.py                1.05      4.5±0.05ms     5.7 MB/sec    1.00      4.3±0.11ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     14.4±0.15ms     2.8 MB/sec    1.01     14.5±0.19ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.06ms     4.4 MB/sec    1.00      3.8±0.03ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    444.4±7.26µs     6.6 MB/sec    1.02    452.7±5.67µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.07ms     3.9 MB/sec    1.00      6.5±0.05ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.01      7.3±0.07ms     5.6 MB/sec    1.00      7.3±0.05ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1484.6±18.20µs    11.2 MB/sec    1.01  1495.4±25.42µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.9±2.53µs    17.9 MB/sec    1.03    169.4±2.36µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.06ms     8.0 MB/sec    1.00      3.2±0.03ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "[WIP] Implement PYI018" to "[`flake8-pyi`] Implement PYI018" by @LaBatata101 on 2023-07-24 22:47_

---

_Marked ready for review by @LaBatata101 on 2023-07-24 23:00_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pyi/snapshots/ruff__rules__flake8_pyi__tests__PYI018_PYI018.pyi.snap`:9 on 2023-07-26 16:08_

Should we highlight the entire declaration?

---

_@zanieb reviewed on 2023-07-26 16:08_

---

_@zanieb reviewed on 2023-07-26 16:09_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pyi/rules/unused_private_type_definition.rs`:37 on 2023-07-26 16:09_

Should we note that the variable is private?

---

_@charliermarsh reviewed on 2023-07-26 16:40_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/snapshots/ruff__rules__flake8_pyi__tests__PYI018_PYI018.pyi.snap`:9 on 2023-07-26 16:40_

I personally think highlighting just the symbol is ok.

---

_@charliermarsh requested changes on 2023-07-26 16:51_

Thank you! I know this follows the suggestions I gave in Discord, but looking at the changes and those involved in the downstream PRs, I want to make a few recommendations for modifying the overall data flow. (If you don't feel up for making these changes given the flip-flopping feedback I've given you, I'd also understand and can make them myself if you prefer.)

Overall, it's probably better to avoid (1) having to create a `BindingFlag` for every unused thing we might care about, and (2) having to add all the detection code ("Is this an assignment to a private type var?") to the semantic model itself.

Instead, I'd like to structure it as follows:

- We can create a single `BindingFlag`, something like `PRIVATE_VARIABLE`, and set that within `SemanticModel#add_binding` (rather than having to set flags at all the `add_binding` call sites throughout `checkers/ast/mod.rs`). This flag would just check if the name is underscore-prefixed.
- In `unused_private_type_var`, check if the kind is an unused assignment to a private variable. If so, check if it's an assignment to a private `TypeVar` using the code you already wrote in `handle_node_store` (you can access the statement that defines the binding via `binding.parent`).

This should also generalize to the other variants we care about, without requiring any changes to the semantic model, only the rules themselves. What do you think?


---

_Comment by @LaBatata101 on 2023-07-26 19:23_

> Thank you! I know this follows the suggestions I gave in Discord, but looking at the changes and those involved in the downstream PRs, I want to make a few recommendations for modifying the overall data flow. (If you don't feel up for making these changes given the flip-flopping feedback I've given you, I'd also understand and can make them myself if you prefer.)
> 
> Overall, it's probably better to avoid (1) having to create a `BindingFlag` for every unused thing we might care about, and (2) having to add all the detection code ("Is this an assignment to a private type var?") to the semantic model itself.
> 
> Instead, I'd like to structure it as follows:
> 
>     * We can create a single `BindingFlag`, something like `PRIVATE_VARIABLE`, and set that within `SemanticModel#add_binding` (rather than having to set flags at all the `add_binding` call sites throughout `checkers/ast/mod.rs`). This flag would just check if the name is underscore-prefixed.
> 
>     * In `unused_private_type_var`, check if the kind is an unused assignment to a private variable. If so, check if it's an assignment to a private `TypeVar` using the code you already wrote in `handle_node_store` (you can access the statement that defines the binding via `binding.parent`).
> 
> 
> This should also generalize to the other variants we care about, without requiring any changes to the semantic model, only the rules themselves. What do you think?

That sounds good, I'll implement it myself. I actually thought about doing something more generic like the `PRIVATE_VARIABLE`  flag, but since I'm not that familiar with the code base yet, and I didn't know how to go about it.

I have one question, what about private classes, like we check in `PYI046`? 
I don't know if  `PRIVATE_VARIABLE` would be fitting, should I create another just for the classes, like `PRIVATE_CLASS`? What do you think?

---

_Review requested from @charliermarsh by @LaBatata101 on 2023-07-26 21:13_

---

_Comment by @charliermarsh on 2023-07-26 22:44_

Great! I renamed to `PRIVATE_DECLARATION` which feels more appropriate when applied to classes and such. I also moved the flag setting into `add_binding`.

---

_@charliermarsh approved on 2023-07-26 22:44_

---

_Label `rule` added by @charliermarsh on 2023-07-26 22:44_

---

_Merged by @charliermarsh on 2023-07-26 22:56_

---

_Closed by @charliermarsh on 2023-07-26 22:56_

---

_Branch deleted on 2023-07-27 21:04_

---
