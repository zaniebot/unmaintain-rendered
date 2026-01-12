```yaml
number: 5123
title: "[Markdown Parser] Add `CodeExtractor` trait for extracting source code from a file"
type: pull_request
state: closed
author: evanrittenhouse
labels: []
assignees: []
base: main
head: 3792_markdown
created_at: 2023-06-15T17:19:10Z
updated_at: 2023-06-20T17:52:05Z
url: https://github.com/astral-sh/ruff/pull/5123
synced_at: 2026-01-12T15:55:17Z
```

# [Markdown Parser] Add `CodeExtractor` trait for extracting source code from a file

---

_@evanrittenhouse_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Will be used when implementing #3792. 

This PR adds a `CodeExtractor` trait (and a Jupyter notebook implementation). This trait will be used to extract source code (language-agnostic, potentially range-agnostic once Open Question 1 is answered) from files. It will be used in a `MarkdownParser` to extract Python code blocks. 
<!-- What's the purpose of the change? What does it do, and why? -->

### Open Questions
1. To support embedded languages (e.g. SQL in Python), we can add an `extract_code_from_range` method, or change the signature of the existing function to take a `str` and pass the in Path contents that way. I'm open to either one, though the latter would obviously require a change in the existing implementation. I think the latter option (changing signature to `str` vs. `Path`) would be better.
2. Do we want to move the `CodeExtractor` trait onto the `SourceKind` enum? I thought about it, but am not sure how that enum can be extended for embedded language support down the road. I think we'd have to change it to work at a "code block" level instead of a `Path` level. My local branch contains the concept of a `BoundedCodeBlock`, but I wonder if that's going to be too much change for now. If there's interest, I can submit the Markdown parser PR with that struct incorporated.

## Test Plan

`cargo t`

## Note
I had a Markdown language parser PR earlier, but there've been a bunch of changes in main (specifically around Jupyter notebooks) that will prove helpful for the Markdown parser, so I'm essentially restarting the branch. Further development on Markdown parsing will be paused until this gets looked at (I think it's currently deprioritized, but just wanted to let it be known :) ).


---

_Review requested from @dhruvmanila by @evanrittenhouse on 2023-06-15 17:19_

---

_Renamed from "Add CodeExtractor trait" to "Add `CodeExtractor` trait" by @evanrittenhouse on 2023-06-15 17:19_

---

_Comment by @evanrittenhouse on 2023-06-15 17:26_

@charliermarsh would also like your thoughts on this, if you get the time.

---

_Comment by @github-actions[bot] on 2023-06-15 17:38_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1397.8±14.74µs    11.9 MB/sec    1.00   1401.3±2.92µs    11.9 MB/sec
formatter/numpy/globals.py                 1.00    137.5±0.25µs    21.5 MB/sec    1.00    137.7±0.40µs    21.4 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.1 MB/sec    1.00      2.8±0.02ms     9.1 MB/sec
linter/all-rules/large/dataset.py          1.02     14.5±0.10ms     2.8 MB/sec    1.00     14.1±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.00ms     4.7 MB/sec    1.00      3.5±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    369.9±3.79µs     8.0 MB/sec    1.00    367.0±0.71µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.03ms     4.1 MB/sec    1.00      6.2±0.04ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.04      7.4±0.03ms     5.5 MB/sec    1.00      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1547.9±4.52µs    10.8 MB/sec    1.00   1512.3±4.36µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    167.4±0.13µs    17.6 MB/sec    1.00    165.8±0.71µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.4±0.02ms     7.6 MB/sec    1.00      3.3±0.01ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.12ms     6.0 MB/sec    1.01      6.9±0.14ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.02  1431.7±58.10µs    11.6 MB/sec    1.00  1406.6±28.53µs    11.8 MB/sec
formatter/numpy/globals.py                 1.00    134.0±3.64µs    22.0 MB/sec    1.04    138.7±9.41µs    21.3 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.06ms     9.1 MB/sec    1.00      2.8±0.07ms     9.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.8±0.28ms     2.8 MB/sec    1.01     14.9±0.37ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.06ms     4.4 MB/sec    1.00      3.8±0.09ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    442.0±9.40µs     6.7 MB/sec    1.02   450.7±12.31µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.17ms     3.9 MB/sec    1.00      6.5±0.15ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.12ms     5.4 MB/sec    1.00      7.5±0.13ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1573.7±32.39µs    10.6 MB/sec    1.01  1593.4±39.03µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.3±4.85µs    16.5 MB/sec    1.02    182.1±5.42µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.06ms     7.5 MB/sec    1.00      3.4±0.07ms     7.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Add `CodeExtractor` trait" to "Add `CodeExtractor` trait for extracting source code from a file" by @evanrittenhouse on 2023-06-15 20:38_

---

_Renamed from "Add `CodeExtractor` trait for extracting source code from a file" to "[Markdown] Add `CodeExtractor` trait for extracting source code from a file" by @evanrittenhouse on 2023-06-17 23:09_

---

_Renamed from "[Markdown] Add `CodeExtractor` trait for extracting source code from a file" to "[Markdown Parser] Add `CodeExtractor` trait for extracting source code from a file" by @evanrittenhouse on 2023-06-17 23:09_

---

_Comment by @charliermarsh on 2023-06-18 15:51_

Thanks, I'll try to take a look later today!

---

_Review comment by @charliermarsh on `crates/ruff/src/source_kind.rs`:34 on 2023-06-19 03:23_

I think the generic can be omitted entirely here with:

```rust
pub trait CodeExtractor: Sized {
    fn extract_code(path: &Path) -> Result<Self, Box<Diagnostic>>;
}
```

---

_@charliermarsh reviewed on 2023-06-19 03:23_

---

_@charliermarsh reviewed on 2023-06-19 03:32_

---

_Review comment by @charliermarsh on `crates/ruff/src/source_kind.rs`:32 on 2023-06-19 03:32_

Maybe I'm unable to see ahead to future usages, but I don't quite see the value in introducing this trait as-is, since it's really just defining a constructor for the implementing class -- there aren't any shared methods across trait implementors (e.g., nothing that takes `&self`), and so implementing the trait doesn't help us very much. How do you see it evolving?

---

_@evanrittenhouse reviewed on 2023-06-19 04:01_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/source_kind.rs`:32 on 2023-06-19 04:01_

I was planning on storing the various mapping functions here as well (like mapping the initial/terminal offsets of Markdown code cells to the underlying document or Jupyter cells vs its JSON). I'm finding a lot of similarities between the Markdown parser and the existing Jupyter one, so wanted to consolidate some functionality. 

Additionally, a lot of these functions will be applicable for embedded language support (mapping initial and terminal offsets of SQL code in Python, for example).

I felt that there was an opportunity to unify a lot of the stuff between the Markdown and Jupyter parsers which will also extend to other use cases in the larger "multi-language" use case. If you don't think this trait is worth it, though, I'm happy to remove it and just continue on the Markdown parser itself. 

---

_@evanrittenhouse reviewed on 2023-06-20 15:23_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/source_kind.rs`:32 on 2023-06-20 15:23_

@charliermarsh I think I'm going to close this for now - you're right. I'll keep working on the Markdown parser and then we can look for opportunities to unify stuff down the road. I'll try to design it in such a way that we can easily implement multi-language support for it down the road.

---

_Closed by @evanrittenhouse on 2023-06-20 15:23_

---

_@charliermarsh reviewed on 2023-06-20 15:28_

---

_Review comment by @charliermarsh on `crates/ruff/src/source_kind.rs`:32 on 2023-06-20 15:28_

Sounds good, sorry for not getting back to your previous comment. I just want to avoid introducing abstractions before they're needed.

---

_Branch deleted on 2023-06-20 17:52_

---
