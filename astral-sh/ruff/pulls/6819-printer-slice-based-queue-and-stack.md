```yaml
number: 6819
title: "Printer: Slice based queue and stack"
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
  - formatter
assignees: []
merged: true
base: main
head: slice-based-queue
created_at: 2023-08-23T13:33:37Z
updated_at: 2023-08-24T12:49:28Z
url: https://github.com/astral-sh/ruff/pull/6819
synced_at: 2026-01-12T15:55:22Z
```

# Printer: Slice based queue and stack

---

_@MichaReiser_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR refactors the `Stack` and `Queue` implementations in the Printer to use `std::slice::Iter` instead of a `slice` + offset pointer. 

This results in a 3%  size reduction:

* Increase in `print_element` -> more aggressive inlining
* `ruff_formatter ruff_formatter::printer::call_stack::CallStack::top_kind` 7 byte reduction (relatively hot)
* `ruff_formatter ruff_formatter::printer::queue::Queue::extend_back` down to a single item and 78 bytes

**Main**
```
File .text    Size           Crate Name
0.0%  0.2%  2.8KiB  ruff_formatter ruff_formatter::printer::Printer::print_element
0.0%  0.1%  2.6KiB  ruff_formatter ruff_formatter::printer::Printer::print_fill_entries
0.0%  0.1%  2.0KiB  ruff_formatter ruff_formatter::printer::FitsMeasurer::fits_element
0.0%  0.1%  1.3KiB  ruff_formatter ruff_formatter::printer::FitsMeasurer::fill_entry_fits
0.0%  0.1%  1.2KiB  ruff_formatter ruff_formatter::printer::Printer::print_with_indent
0.0%  0.0%    891B  ruff_formatter ruff_formatter::printer::queue::Queue::skip_content
0.0%  0.0%    792B  ruff_formatter ruff_formatter::printer::Printer::print_entry
0.0%  0.0%    740B  ruff_formatter ruff_formatter::printer::Printer::fits
0.0%  0.0%    674B  ruff_formatter ruff_formatter::printer::Printer::flat_group_print_mode
0.0%  0.0%    595B  ruff_formatter ruff_formatter::printer::FitsMeasurer::fits_text
0.0%  0.0%    508B  ruff_formatter ruff_formatter::printer::Printer::print_text
0.0%  0.0%    458B  ruff_formatter ruff_formatter::printer::Printer::flush_line_suffixes
0.0%  0.0%    364B  ruff_formatter ruff_formatter::printer::FitsMeasurer::fits_group
0.0%  0.0%    361B  ruff_formatter ruff_formatter::printer::Printer::print_char
0.0%  0.0%    291B  ruff_formatter ruff_formatter::printer::queue::Queue::extend_back
0.0%  0.0%    289B ruff_formatter? <ruff_formatter::printer::queue::QueueContentIterator<Q> as core::iter::traits::iterator::Iterator>::next
0.0%  0.0%    223B  ruff_formatter ruff_formatter::printer::queue::Queue::extend_back
0.0%  0.0%    189B  ruff_formatter ruff_formatter::printer::line_suffixes::LineSuffixes::extend
0.0%  0.0%    179B  ruff_formatter ruff_formatter::printer::GroupModes::insert_print_mode
0.0%  0.0%    165B  ruff_formatter ruff_formatter::printer::Printer::push_marker
0.0%  0.0%    144B  ruff_formatter ruff_formatter::printer::FitsMeasurer::finish
0.0%  0.0%    138B             std core::ptr::drop_in_place<ruff_formatter::printer::Printer>
0.0%  0.0%    132B  ruff_formatter ruff_formatter::printer::Printer::print_str
0.0%  0.0%    101B             std core::ptr::drop_in_place<alloc::vec::drain::Drain<ruff_formatter::printer::line_suffixes::LineSuffixEntry>>
0.0%  0.0%    101B             std core::ptr::drop_in_place<core::iter::adapters::rev::Rev<alloc::vec::drain::Drain<ruff_formatter::printer::line_suffixes::LineSuffixEntry>>>
0.0%  0.0%     79B  ruff_formatter ruff_formatter::printer::call_stack::CallStack::push
0.0%  0.0%     77B  ruff_formatter ruff_formatter::printer::call_stack::CallStack::push
0.0%  0.0%     75B  ruff_formatter ruff_formatter::printer::call_stack::CallStack::top_kind
0.0%  0.0%     62B  ruff_formatter ruff_formatter::printer::invalid_start_tag
0.0%  0.0%     62B  ruff_formatter ruff_formatter::printer::invalid_start_tag
0.0%  0.0%     62B  ruff_formatter ruff_formatter::printer::invalid_start_tag
0.0%  0.0%     41B             std core::ptr::drop_in_place<ruff_formatter::printer::FitsMeasurer>
0.0%  0.0%      0B                 And 0 smaller methods. Use -n N to show more.
0.3%  1.0% 17.4KiB                 filtered data size, the file size is 5.5MiB
```

**This PR**

```
0.1%  0.2%  3.0KiB  ruff_formatter ruff_formatter::printer::Printer::print_element
0.0%  0.1%  2.4KiB  ruff_formatter ruff_formatter::printer::Printer::print_fill_entries
0.0%  0.1%  2.0KiB  ruff_formatter ruff_formatter::printer::FitsMeasurer::fits_element
0.0%  0.1%  1.1KiB  ruff_formatter ruff_formatter::printer::Printer::print_with_indent
0.0%  0.1%  1.0KiB  ruff_formatter ruff_formatter::printer::FitsMeasurer::fill_entry_fits
0.0%  0.0%    719B  ruff_formatter ruff_formatter::printer::Printer::print_entry
0.0%  0.0%    700B  ruff_formatter ruff_formatter::printer::Printer::flat_group_print_mode
0.0%  0.0%    634B  ruff_formatter ruff_formatter::printer::Printer::flush_line_suffixes
0.0%  0.0%    595B  ruff_formatter ruff_formatter::printer::FitsMeasurer::fits_text
0.0%  0.0%    583B  ruff_formatter ruff_formatter::printer::queue::Queue::skip_content
0.0%  0.0%    522B  ruff_formatter ruff_formatter::printer::Printer::fits
0.0%  0.0%    508B  ruff_formatter ruff_formatter::printer::Printer::print_text
0.0%  0.0%    433B ruff_formatter? <ruff_formatter::printer::queue::QueueContentIterator<Q> as core::iter::traits::iterator::Iterator>::next
0.0%  0.0%    364B  ruff_formatter ruff_formatter::printer::FitsMeasurer::fits_group
0.0%  0.0%    361B  ruff_formatter ruff_formatter::printer::Printer::print_char
0.0%  0.0%    271B  ruff_formatter <ruff_formatter::printer::queue::FitsQueue as ruff_formatter::printer::queue::Queue>::pop
0.0%  0.0%    189B  ruff_formatter ruff_formatter::printer::line_suffixes::LineSuffixes::extend
0.0%  0.0%    179B  ruff_formatter ruff_formatter::printer::GroupModes::insert_print_mode
0.0%  0.0%    165B  ruff_formatter ruff_formatter::printer::Printer::push_marker
0.0%  0.0%    138B             std core::ptr::drop_in_place<ruff_formatter::printer::Printer>
0.0%  0.0%    135B  ruff_formatter ruff_formatter::printer::FitsMeasurer::finish
0.0%  0.0%    132B  ruff_formatter ruff_formatter::printer::Printer::print_str
0.0%  0.0%    101B             std core::ptr::drop_in_place<alloc::vec::drain::Drain<ruff_formatter::printer::line_suffixes::LineSuffixEntry>>
0.0%  0.0%    101B             std core::ptr::drop_in_place<core::iter::adapters::rev::Rev<alloc::vec::drain::Drain<ruff_formatter::printer::line_suffixes::LineSuffixEntry>>>
0.0%  0.0%     79B  ruff_formatter ruff_formatter::printer::call_stack::CallStack::push
0.0%  0.0%     78B  ruff_formatter <ruff_formatter::printer::queue::PrintQueue as ruff_formatter::printer::queue::Queue>::extend_back
0.0%  0.0%     77B  ruff_formatter ruff_formatter::printer::call_stack::CallStack::push
0.0%  0.0%     68B  ruff_formatter ruff_formatter::printer::call_stack::CallStack::top_kind
0.0%  0.0%     62B  ruff_formatter ruff_formatter::printer::invalid_start_tag
0.0%  0.0%     62B  ruff_formatter ruff_formatter::printer::invalid_start_tag
0.0%  0.0%     62B  ruff_formatter ruff_formatter::printer::invalid_start_tag
0.0%  0.0%     40B             std core::ptr::drop_in_place<ruff_formatter::printer::FitsMeasurer>
0.0%  0.0%      0B                 And 0 smaller methods. Use -n N to show more.
0.3%  1.0% 16.9KiB                 filtered data size, the file size is 5.5MiB
```

## Test Plan

`cargo test`

---

_Comment by @MichaReiser on 2023-08-23 13:33_

Current dependencies on/for this PR:
* main
  * **PR #6814** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6814" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.svg" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #6815** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6815" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.svg" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #6816** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6816" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.svg" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #6817** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6817" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.svg" alt="Graphite" width="10px" height="10px"/></a> 
          * **PR #6848** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6848" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.svg" alt="Graphite" width="10px" height="10px"/></a> 
          * **PR #6819** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6819" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.svg" alt="Graphite" width="10px" height="10px"/></a>  👈

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/6819?utm_source=stack-comment).

---

_Comment by @github-actions[bot] on 2023-08-23 14:24_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      4.8±0.02ms     8.6 MB/sec    1.00      4.7±0.02ms     8.6 MB/sec
formatter/numpy/ctypeslib.py               1.01   1002.2±5.85µs    16.6 MB/sec    1.00    987.6±3.86µs    16.9 MB/sec
formatter/numpy/globals.py                 1.03     96.3±0.83µs    30.6 MB/sec    1.00     93.6±0.48µs    31.5 MB/sec
formatter/pydantic/types.py                1.00   1951.7±8.18µs    13.1 MB/sec    1.00   1942.8±5.89µs    13.1 MB/sec
linter/all-rules/large/dataset.py          1.02     12.4±0.09ms     3.3 MB/sec    1.00     12.2±0.08ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.00      3.3±0.03ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    462.1±0.90µs     6.4 MB/sec    1.00    460.7±0.76µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.4±0.06ms     4.0 MB/sec    1.00      6.4±0.04ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      6.6±0.02ms     6.1 MB/sec    1.00      6.5±0.03ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1452.8±6.97µs    11.5 MB/sec    1.00   1442.3±2.86µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.4±1.24µs    17.3 MB/sec    1.00    169.8±0.90µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.1±0.17ms     8.0 MB/sec    1.02      5.2±0.27ms     7.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   996.2±15.78µs    16.7 MB/sec    1.01  1004.7±27.16µs    16.6 MB/sec
formatter/numpy/globals.py                 1.00     97.2±5.83µs    30.4 MB/sec    1.02     98.8±7.87µs    29.9 MB/sec
formatter/pydantic/types.py                1.00      2.0±0.04ms    12.7 MB/sec    1.00      2.0±0.07ms    12.7 MB/sec
linter/all-rules/large/dataset.py          1.01     13.3±0.39ms     3.1 MB/sec    1.00     13.1±0.41ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.14ms     4.6 MB/sec    1.00      3.6±0.15ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   440.7±12.02µs     6.7 MB/sec    1.00    441.0±8.99µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.10      7.3±0.34ms     3.5 MB/sec    1.00      6.7±0.16ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.21ms     5.6 MB/sec    1.00      7.3±0.23ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1531.5±41.00µs    10.9 MB/sec    1.04  1586.3±29.20µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    177.8±4.32µs    16.6 MB/sec    1.03    182.6±3.94µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.08ms     7.6 MB/sec    1.03      3.5±0.10ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `performance` added by @MichaReiser on 2023-08-23 15:53_

---

_Label `formatter` added by @MichaReiser on 2023-08-23 15:53_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-08-24 09:57_

---

_Marked ready for review by @MichaReiser on 2023-08-24 10:31_

---

_Comment by @konstin on 2023-08-24 11:59_

The CI benchmarks look like a perf regression

---

_Comment by @MichaReiser on 2023-08-24 12:09_

> The CI benchmarks look like a perf regression

I need to merge the baseline PR first. I believe our benchmark always compares against main

---

_@konstin approved on 2023-08-24 12:25_

lgtm, given that perf doesn't regress anymore with the new numbers from main

---

_Merged by @MichaReiser on 2023-08-24 12:49_

---

_Closed by @MichaReiser on 2023-08-24 12:49_

---

_Branch deleted on 2023-08-24 12:49_

---
