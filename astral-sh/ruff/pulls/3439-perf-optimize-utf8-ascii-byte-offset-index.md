```yaml
number: 3439
title: "perf: Optimize UTF8/ASCII byte offset index"
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: perf/byte-index
created_at: 2023-03-10T13:39:36Z
updated_at: 2023-03-11T12:12:12Z
url: https://github.com/astral-sh/ruff/pull/3439
synced_at: 2026-01-12T04:39:44Z
```

# perf: Optimize UTF8/ASCII byte offset index

---

_Pull request opened by @MichaReiser on 2023-03-10 13:39_

This PR improves the memory consumption and performance of our line/column based `Location`s to byte offsets. The PR also introduces two new zero-cost wrappers so that the functionality can be implemented as methods rather than free floating functions. 

The first improvement is that the implementation no longer performs two passes over the string:

1. To determine if its ASCII
2. To build up the index

The implementation now assumes ASCII and falls back (and converts the intermediate state) to UTF8 when it sees the first non-ascii character while building the index. 

The second optimization is to use `u32` to store the offsets (has anyone ever tried running a 4GB+ source document with Python?)

The last optimization avoids the nested `Vec<Vec<usize>>` for the `Utf8Index`. I tried two different approaches

## Line -> Char and Char -> Byte Index

The idea is to use two vectors instead of a nested vector:

* The first vector stores the *character* offset at which a new line starts
* The second vector stores the *byte* offsets for each character

You can then compute the [Location] offset by:

```rust
// retrieving the start character on that line and add the column (character offset)
let char_offset = lines[location.row() - 1] + location.column();
let byte_offset = char_to_byte_offsets[char_offset]
```
0b39da539059d87301f7319a2575114d87363e49

## Lazy column computation

This implementation makes the assumption that we're only querying a few locations and that computing the column offset for a single line is cheap (no minified documents where everything is on a single line). 

Based on this assumption, it is sufficient to only store the byte offsets for every line and then lazily compute the column offset. 

Computing the index lazily has the added benefit of reducing the memory footprint. 

7527612fcd254a14fcba1fa6d34a7d15883802e9

## Benchmark

```
❯ hyperfine --ignore-failure --warmup 10 \
        "./ruff_two ./crates/ruff/resources/test/cpython/ --no-cache" \
        "./ruff_main ./crates/ruff/resources/test/cpython/ --no-cache" \
        "./ruff_lazy ./crates/ruff/resources/test/cpython/ --no-cache"
Benchmark 1: ./ruff_two ./crates/ruff/resources/test/cpython/ --no-cache
  Time (mean ± σ):     186.3 ms ±   3.5 ms    [User: 3177.1 ms, System: 91.2 ms]
  Range (min … max):   180.5 ms … 192.1 ms    15 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ./ruff_main ./crates/ruff/resources/test/cpython/ --no-cache
  Time (mean ± σ):     187.8 ms ±   3.5 ms    [User: 3215.8 ms, System: 99.4 ms]
  Range (min … max):   183.9 ms … 195.6 ms    15 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 3: ./ruff_lazy ./crates/ruff/resources/test/cpython/ --no-cache
  Time (mean ± σ):     184.7 ms ±   4.1 ms    [User: 3188.5 ms, System: 97.6 ms]
  Range (min … max):   179.5 ms … 195.8 ms    16 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  './ruff_lazy ./crates/ruff/resources/test/cpython/ --no-cache' ran
    1.01 ± 0.03 times faster than './ruff_two ./crates/ruff/resources/test/cpython/ --no-cache'
    1.02 ± 0.03 times faster than './ruff_main ./crates/ruff/resources/test/cpython/ --no-cache'

```

## Memory usage

I used `/usr/bin/time` to get the max resident memory when linting cpython

* `main`:  ~324MB
* `two`: ~298MB
* `lazy`: ~287MB

## Verdict

This PR implements the `lazy` computation as it has a similar performance as storing all character positions but requires significantly less memory (only stores line mappings, not also mappings for every line)

---

_Marked ready for review by @MichaReiser on 2023-03-10 16:30_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/locator.rs`:68 on 2023-03-10 16:47_

Nit: there are a couple `[Location]` usages that may need to be wrapped in backticks.

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/locator.rs`:99 on 2023-03-10 16:49_

`Self::Utf8` for consistency with line 108?

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/locator.rs`:92 on 2023-03-10 16:50_

(For context, 48 was chosen based on some rough benchmarking of various constants, so it's sort of arbitrary but not totally arbitrary.)

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/locator.rs`:129 on 2023-03-10 16:51_

Does this now properly handle CR line endings for UTF-8 files? (We don't support them right now which often leads to panics.)

---

_@charliermarsh approved on 2023-03-10 16:52_

These are nice improvements!

---

_@MichaReiser reviewed on 2023-03-11 09:05_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/locator.rs`:129 on 2023-03-11 09:05_

It should. Let me add a test and verify that ASCII handles `\r` correctly too

---

_Comment by @github-actions[bot] on 2023-03-11 10:47_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Merged by @MichaReiser on 2023-03-11 12:12_

---

_Closed by @MichaReiser on 2023-03-11 12:12_

---

_Branch deleted on 2023-03-11 12:12_

---
