```yaml
number: 4288
title: u32-backed OneIndexed
type: pull_request
state: closed
author: youknowone
labels: []
assignees: []
base: main
head: one-indexed-u32
created_at: 2023-05-08T18:49:18Z
updated_at: 2023-05-12T07:07:08Z
url: https://github.com/astral-sh/ruff/pull/4288
synced_at: 2026-01-12T15:55:15Z
```

# u32-backed OneIndexed

---

_@youknowone_

I tried to minimize diagnostic side code changes.

The key change of interface is `to_one_indexed`. By adding this method, the interface consistently requires to use `one_indexed` or `zero_indexed` to convert `OneIndexed` to usize or vice versa.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-08 18:49_

---

_Comment by @github-actions[bot] on 2023-05-08 19:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     17.7±0.86ms     2.3 MB/sec    1.00     17.2±0.83ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.16ms     4.0 MB/sec    1.02      4.2±0.20ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.06   527.5±12.51µs     5.6 MB/sec    1.00   498.3±20.29µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.09      7.4±0.23ms     3.5 MB/sec    1.00      6.8±0.22ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.05      9.0±0.51ms     4.5 MB/sec    1.00      8.5±0.53ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1896.1±52.81µs     8.8 MB/sec    1.00  1878.5±117.04µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    213.9±6.58µs    13.8 MB/sec    1.00   213.2±11.76µs    13.8 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.1±0.16ms     6.3 MB/sec    1.00      3.9±0.20ms     6.5 MB/sec
parser/large/dataset.py                    1.00      6.8±0.39ms     6.0 MB/sec    1.06      7.1±0.19ms     5.7 MB/sec
parser/numpy/ctypeslib.py                  1.02  1367.8±66.80µs    12.2 MB/sec    1.00  1336.0±67.91µs    12.5 MB/sec
parser/numpy/globals.py                    1.00    125.9±6.13µs    23.4 MB/sec    1.07    134.2±5.96µs    22.0 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.16ms     9.0 MB/sec    1.04      3.0±0.10ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     21.1±0.53ms  1971.5 KB/sec    1.00     20.9±0.50ms  1994.4 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.4±0.27ms     3.1 MB/sec    1.00      5.3±0.22ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   622.8±20.52µs     4.7 MB/sec    1.01   627.5±21.85µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.9±0.28ms     2.9 MB/sec    1.00      8.7±0.26ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.02     10.7±0.23ms     3.8 MB/sec    1.00     10.5±0.22ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.07ms     7.5 MB/sec    1.01      2.2±0.08ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.01   252.8±17.13µs    11.7 MB/sec    1.00   250.8±12.22µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.7±0.14ms     5.4 MB/sec    1.00      4.7±0.15ms     5.4 MB/sec
parser/large/dataset.py                    1.01      8.7±0.18ms     4.7 MB/sec    1.00      8.6±0.19ms     4.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1657.4±54.60µs    10.0 MB/sec    1.02  1685.8±64.47µs     9.9 MB/sec
parser/numpy/globals.py                    1.00    171.6±9.39µs    17.2 MB/sec    1.00   171.5±13.75µs    17.2 MB/sec
parser/pydantic/types.py                   1.00      3.7±0.09ms     6.9 MB/sec    1.03      3.8±0.24ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/line_index.rs`:262 on 2023-05-09 07:58_

We should implement `TryFrom<usize>` and require the caller to explicitly handle the case where `value` is larger than a `u32`. While rare, it can happen for a 4GB file with all content on a single line (which can be represented by `TextSize` but not by `NonZeroU32` because we start counting at 1. 

The other reason why explicitly requiring the caller to handle potential overflows is because Ruff should never panic rendering `Diagnostic`s. We are probably not defensive enough about this today (e.g. an incorrect `TextRange` causes a panic) but that's where we ultimately want to get. 

The other reason why explicitly asking the caller to handle the potential failure is that `OneIndexed` is not related to `TextSize`. Meaning, it can be used to represent location into any document, even documents that haven't been parsed by RustPython. 

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/line_index.rs`:279 on 2023-05-09 08:00_

I think we should change the argument here to a `u32` too, to ensure the whole API either consistently uses `u32` or `usize`s. For that reason, I would also recommend removing `to_one_indexed`. What we can do is implement `impl  From<OneIndexed> for usize`. 

---

_Review comment by @youknowone on `crates/ruff_python_ast/src/source_code/line_index.rs`:271 on 2023-05-09 08:06_

This is unrelated to the described change. I suggest to remove `pub` here or rename `ONE` to something other like `NONZERO_ONE`. The name `ONE` makes it looks like `Self` type, but it isn't.


---

_@youknowone reviewed on 2023-05-09 08:06_

---

_@MichaReiser reviewed on 2023-05-09 09:13_

Thanks for this contribution. 

I am conflicted about whether this is worth changing. My first draft of `OneIndex` used `NonZeroU32` internally but I then decided to change it to `NonZeroUsize` because:

* It is easier to use (no panics, `unwrap`, casts or conversions)
* Other than `TextSize`, `OneIndexed` isn't used in any performant critical path where optimizing its size matters. The only place that comes to my mind that may improve from this change is when using the `JsonEmitter` with a large number of `Diagnostic`s, but I would reason that the lookup of the source location is the dominating performance factor. 
* Assertions (potential panics) have a small runtime cost. 

What was your motivation for this change? Is this infrastructure that you plan to ship with RustPython and you want to make sure they're aligned? 



---

_Review comment by @youknowone on `crates/ruff_python_ast/src/source_code/line_index.rs`:262 on 2023-05-09 10:14_

I got the second reason, but have a question about the first reason.

What happens if source code is bigger than 4GB? Because TextSize is backed by u32, I thought source code bigger than 4GB will not be located.
Does ruff use different mechanism to solve large files?
Whether it is or not, the exact 4GB file seem to be handled in same way as 4GB+ files.

----

Not strictly related to this topic, but some platforms even take 64bit file size when the underlying architecture is 32bit. Source code length can be not limited to u32. (Though I hope nobody tries to make 4GB source code in single file)

---

_Review comment by @youknowone on `crates/ruff_python_ast/src/source_code/line_index.rs`:279 on 2023-05-09 10:38_

I agree making this function to take u32 is better.

About `to_one_indexed`, I will follow your decision, but I'd like to add one more comment.

Unlike `NonZero` type, which means zero is out of range, `OneIndexed` offers conversion functions to zero indexed value. I also can say `OneIndexed` is another representation of zero indexed value.
When user convert this value to usize, sometimes it is conversion to underlying value (zero-indexed) and something it is conversion to representation (one-indexed).
By providing two symmetric interfaces, users can always select their intentions with equal weight by looking at the function names. It helps to reduce mistakes.
By providing `From<OneIndexed> for usize`, users tends to regards one way is default behavior and the other way is optional function.  It may be the truth because this type is designed to hold one-indexed values. But optional function is easy to be forgotten.
Practically it might be confusing. It will not be confused in one way - using to_zero_indexed is very explicit, but the other way - writing `value.into()` for `usize` conversion - is very easy to happen inconsiously.

Ruff codebase is using either conversion in similar ratio in small numbers - to-zero 8 and to-one 7. Implementing `From<OneIndexed> for usize` doesn't give much value and also hard to say which conversion is dominant. I personally will not support that `From` implementation while it supports both one and zero indexed conversions.

---

_@youknowone reviewed on 2023-05-09 10:38_

---

_@MichaReiser reviewed on 2023-05-09 11:19_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/line_index.rs`:262 on 2023-05-09 11:19_

> What happens if source code is bigger than 4GB? Because TextSize is backed by u32, I thought source code bigger than 4GB will not be located.

Ruff will panic for files larger than 4GB. We should probably add a nice diagnostic right before parsing the file to gracefully handle this case.

The main point here is that `TextSize` can handle files with a length up to `u32::MAX`, but `OneIndexed` only up to `u32::MAX - 1`.

---

_@MichaReiser reviewed on 2023-05-09 11:23_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/line_index.rs`:279 on 2023-05-09 11:23_

> About to_one_indexed, I will follow your decision, but I'd like to add one more comment.

That's an excellent point. I guess what I find confusing is that both `get` and `to_one_indexed` return the same value and only differ by the return type. What do you think of `get` and `get_usize()` (or `to_one_indexed()` and `to_one_indexed_usize()`?

---

_@youknowone reviewed on 2023-05-09 11:23_

---

_Review comment by @youknowone on `crates/ruff_python_ast/src/source_code/line_index.rs`:262 on 2023-05-09 11:23_

That's exactly what I meant to. If ruff reject 4GB+ files, ruff can reject the exact 4GB file too when reading file if length  = u32::MAX. If ruff is going to add a nice diagnostic, it can add the same thing by checking length == u32::MAX when reading file length.
It will simplify exception handling just by excluding one very rare edge case.

---

_Review comment by @youknowone on `crates/ruff_python_ast/src/source_code/line_index.rs`:279 on 2023-05-09 11:25_

I thought `get()` returns underlying value and `to_one_indexed` and `to_zero_indexed` returns either indexes.
To avoid `get`, my suggestion is `to_u32() -> u32`, `to_one_indexed() -> usize` and `to_zero_indexed( ) -> usize`.


---

_@youknowone reviewed on 2023-05-09 11:25_

---

_@youknowone reviewed on 2023-05-09 12:52_

---

_Review comment by @youknowone on `crates/ruff_python_ast/src/source_code/line_index.rs`:279 on 2023-05-09 12:52_

I tried to edit my comment but it doesn't work due to github problem 😂 

Otherwise, just full matrix also makes sense to me. Basically same as your idea, but adding another function.

`to_one_indexed() -> u32`
`to_zero_indexed() -> u32`
`to_one_indexed_usize() -> usize`
`to_zero_indexed_usize() -> usize`

or 

`to_one_indexed_u32() -> u32`
`to_zero_indexed_u32() -> u32`
`to_one_indexed() -> usize`
`to_zero_indexed() -> usize`

To my sight, using 3 functions looks like underlying data getter + index conversions. To keep 3 functions, returning usize looks better for conversion functions. Otherwise `get` and `to_one_indexed` will not be distinguished and misused. Using 4 functions looks like all the conversion functions. Anything looks fine for this.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/line_index.rs`:279 on 2023-05-09 13:00_

`get` returns the underlining value (which is one indexed by design), similar to [`NonZeroUsize::get`](https://doc.rust-lang.org/stable/std/num/struct.NonZeroUsize.html#method.get). 

I guess keeping `get` and adding a `to_usize` or `to_u32` makes sense to me. But we should ensure that the API s consistent (works on `u32` or `usize`) 

---

_@MichaReiser reviewed on 2023-05-09 13:00_

---

_Review comment by @youknowone on `crates/ruff/src/message/diff.rs`:59 on 2023-05-09 13:36_

I left error case as unwrap. You will be able to fill proper handling later.

---

_Review comment by @youknowone on `crates/ruff_python_ast/src/source_code/line_index.rs`:310 on 2023-05-09 13:42_

Added a new method not to use `as usize` everywhere.

---

_@youknowone reviewed on 2023-05-09 13:42_

---

_@MichaReiser requested changes on 2023-05-10 07:19_

Could you explain your main motivation behind my change? 

I would prefer not merging this PR ([see comment](https://github.com/charliermarsh/ruff/pull/4288#pullrequestreview-1418091123)) if performance is the main reason because it increases the complexity of the API, adds more paths that can potentially panic without it measurably increasing performance (because we don't use `SourceLocation` in a hot path).

---

_@youknowone reviewed on 2023-05-10 10:58_

I needed to work on this changes and wanted to contribute it back because it seems worth (even though not that much).
It doesn't need to be aligned. RustPython borrowed a copy of code but it is never used in Ruff side.

For panic points view,
You said `OneIndexed` can be used for non-text-size context, but it was always used for text size directly or indirectly as I've checked `unwrap()` parts.
The main source of unwrap came from TextDiff. I checked it is external dependency but the value will be always under TextSize range because it takes source code limited by TextSize (well, not that confident tbh). Well, the right way to handling this will be separating OneIndexedU32 and OneIndexed(usize). You may not like it because it is unnecessary code expansion for non-critical path.

I agree the change doesn't make sufficient advantage to the project. Please feel free to close it if you think it doesn't fit in the project. 

---

_Comment by @MichaReiser on 2023-05-12 07:07_

I'll close this for now because it increases the complexity of the API without improving performance, memory consumption significantly. 

We should reconsider this change if we start storing many `SourceLocation` nodes or if aligning with `RustPython`'s `SourceLocation` implementation removes the need for Ruff to have it's own fork or eases pulling in upstream changes.

---

_Closed by @MichaReiser on 2023-05-12 07:07_

---
