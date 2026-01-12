```yaml
number: 3897
title: Store source code on message
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: store-content-on-message
created_at: 2023-04-06T10:18:38Z
updated_at: 2023-04-11T08:13:35Z
url: https://github.com/astral-sh/ruff/pull/3897
synced_at: 2026-01-12T04:28:19Z
```

# Store source code on message

---

_Pull request opened by @MichaReiser on 2023-04-06 10:18_

This PR stores the whole source text on `Message` so that it becomes possible to build up a code frame with dynamic context or show a diff for the code changes. 

The PR introduces two new data structures

* `SourceCode`: Similar to `Locator`. It stores a reference to the source text and a reference to a `LineIndex`. The difference to `Locator` is that the `LineIndex` is eagerly computed when creating the `SourceCode`
* `SourceCodeBuf`: A owned version of `SourceCode` that uses an `Arc<str>` for the source code to be cheaply cloneable. 

`SourceCode` is used by both `SourceCodeBuf` and `Locator` to implement the "resolution" logic that is shared between all three structs. 

## Serialization

The `Message` serialization in ruff's cache skips over the `source` field when serializing the messages to avoid serializing the content `n` times. Instead, the serializer computes the unique contents using the file name and stores each content only once. 

## Benchmark

This change improves/regresses the runtime of some configurations slightly.

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `./target/release/ruff ./crates/ruff/resources/test/cpython/ -e   ` | 37.5 ± 1.9 | 33.6 | 40.9 | 1.00 |
| `./ruff-main ./crates/ruff/resources/test/cpython/ -e   ` | 37.6 ± 1.6 | 33.9 | 40.7 | 1.00 ± 0.07 |
| `./target/release/ruff ./crates/ruff/resources/test/cpython/ -e --no-cache  ` | 209.1 ± 3.2 | 201.9 | 213.5 | 5.57 ± 0.29 |
| `./ruff-main ./crates/ruff/resources/test/cpython/ -e --no-cache  ` | 212.2 ± 5.5 | 202.7 | 223.5 | 5.65 ± 0.32 |
| `./target/release/ruff ./crates/ruff/resources/test/cpython/ -e  --select=ALL ` | 439.5 ± 10.3 | 422.0 | 456.2 | 11.71 ± 0.64 |
| `./ruff-main ./crates/ruff/resources/test/cpython/ -e  --select=ALL ` | 431.3 ± 9.4 | 410.9 | 447.3 | 11.49 ± 0.62 |
| `./target/release/ruff ./crates/ruff/resources/test/cpython/ -e --no-cache --select=ALL ` | 745.9 ± 14.2 | 722.4 | 768.2 | 19.87 ± 1.05 |
| `./ruff-main ./crates/ruff/resources/test/cpython/ -e --no-cache --select=ALL ` | 749.6 ± 9.4 | 738.5 | 763.2 | 19.97 ± 1.02 |
| `./target/release/ruff ./crates/ruff/resources/test/cpython/ -e   --show-source` | 66.9 ± 2.2 | 63.7 | 73.8 | 1.78 ± 0.11 |
| `./ruff-main ./crates/ruff/resources/test/cpython/ -e   --show-source` | 66.4 ± 2.2 | 63.0 | 71.7 | 1.77 ± 0.11 |
| `./target/release/ruff ./crates/ruff/resources/test/cpython/ -e --no-cache  --show-source` | 234.1 ± 4.2 | 227.4 | 240.4 | 6.24 ± 0.33 |
| `./ruff-main ./crates/ruff/resources/test/cpython/ -e --no-cache  --show-source` | 235.9 ± 6.0 | 229.0 | 249.0 | 6.28 ± 0.35 |
| `./target/release/ruff ./crates/ruff/resources/test/cpython/ -e  --select=ALL --show-source` | 1047.8 ± 16.1 | 1021.9 | 1073.2 | 27.91 ± 1.45 |
| `./ruff-main ./crates/ruff/resources/test/cpython/ -e  --select=ALL --show-source` | 1041.8 ± 10.1 | 1024.6 | 1056.5 | 27.75 ± 1.40 |
| `./target/release/ruff ./crates/ruff/resources/test/cpython/ -e --no-cache --select=ALL --show-source` | 1359.4 ± 19.6 | 1334.5 | 1386.7 | 36.21 ± 1.87 |
| `./ruff-main ./crates/ruff/resources/test/cpython/ -e --no-cache --select=ALL --show-source` | 1358.1 ± 30.6 | 1335.4 | 1435.3 | 36.17 ± 1.97 |





---

_Comment by @MichaReiser on 2023-04-06 10:18_

Current dependencies on/for this PR:
* main
  * **PR #3895** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3895" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #3896** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3896" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #3897** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3897" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈
        * **PR #3901** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3901" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
          * **PR #3904** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3904" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
            * **PR #3905** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3905" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
              * **PR #3906** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3906" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/charliermarsh/ruff/3897?utm_source=stack-comment).

---

_Comment by @github-actions[bot] on 2023-04-06 10:33_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.05ms     2.9 MB/sec    1.01     14.2±0.08ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    456.7±1.29µs     6.5 MB/sec    1.00    451.8±1.49µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.02ms     4.2 MB/sec    1.00      6.0±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.02ms     5.7 MB/sec    1.01      7.2±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1616.1±5.75µs    10.3 MB/sec    1.00   1620.4±5.21µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    180.5±0.44µs    16.4 MB/sec    1.00    178.9±0.50µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.00ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.4±0.56ms     2.2 MB/sec    1.02     18.9±0.59ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      4.9±0.17ms     3.4 MB/sec    1.00      4.6±0.18ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.02   583.5±21.62µs     5.1 MB/sec    1.00   569.6±26.61µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.03      8.1±0.17ms     3.2 MB/sec    1.00      7.8±0.25ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      9.6±0.24ms     4.3 MB/sec    1.00      9.5±0.27ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.1±0.10ms     7.9 MB/sec    1.00      2.1±0.09ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   220.8±10.68µs    13.4 MB/sec    1.03   226.3±12.58µs    13.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.16ms     6.1 MB/sec    1.03      4.4±0.39ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @MichaReiser on 2023-04-06 12:58_

---

_@charliermarsh reviewed on 2023-04-06 21:55_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/mod.rs`:53 on 2023-04-06 21:55_

A bold rename!

---

_@charliermarsh reviewed on 2023-04-06 21:56_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/mod.rs`:174 on 2023-04-06 21:56_

A bold rename!

---

_@charliermarsh reviewed on 2023-04-06 21:57_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/cache.rs`:117 on 2023-04-06 21:57_

Why remove this generic? (Not objecting, looking to learn.)

---

_@charliermarsh reviewed on 2023-04-06 21:58_

---

_Review comment by @charliermarsh on `crates/ruff/src/message/grouped.rs`:117 on 2023-04-06 21:58_

Imported here for shadowing, to avoid the need to alias?

---

_@charliermarsh reviewed on 2023-04-06 21:59_

---

_Review comment by @charliermarsh on `crates/ruff/src/linter.rs`:374 on 2023-04-06 21:59_

I wonder if we can apply some similar efficiency tricks to the path...? This also gets cloned and serialized once per diagnostic.

---

_@charliermarsh approved on 2023-04-06 21:59_

---

_@MichaReiser reviewed on 2023-04-07 10:12_

---

_Review comment by @MichaReiser on `crates/ruff/src/linter.rs`:374 on 2023-04-07 10:12_

Good idea. See #3904 (I created a PR on top to minimize the time spent rebasing the stacked PRs).

---

_@MichaReiser reviewed on 2023-04-07 10:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/mod.rs`:53 on 2023-04-07 10:14_

😅 I don't feel too strongly about it. I just found that `after` works better with `TextRange::up_to`. 

---

_@MichaReiser reviewed on 2023-04-07 10:16_

---

_Review comment by @MichaReiser on `crates/ruff/src/message/grouped.rs`:117 on 2023-04-07 10:16_

Yeah. The alternative is to use `use std::fmt::{Write as WriteFmt}` at the top. I guess it was just easier to type it here than jumping to the top and manually at the use

---

_@MichaReiser reviewed on 2023-04-07 10:19_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:117 on 2023-04-07 10:19_

I can revert the change if you want. 
I experimented with reading the `source` in `get` instead of serializing it to the cache (because it seems a bit unnecessary) but then forgot to undo the change when I decided to not pursue this approach. 

The problems I ran into is are that `P` is not `Copy` and the constraint that `P` for `path` and `project_dir` must be of the same type was also too restricting. 

---

_@charliermarsh reviewed on 2023-04-07 13:21_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/mod.rs`:53 on 2023-04-07 13:21_

Hah me neither. I had used `until` and `after` to mimic the iterator API but I genuinely don't feel strongly, trust you to do whatever makes sense.

---

_@charliermarsh reviewed on 2023-04-07 13:24_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/cache.rs`:117 on 2023-04-07 13:24_

Don't feel strongly, up to you!

---

_Comment by @jfmengels on 2023-04-08 09:20_

@MichaReiser Am I understanding correctly that this will also make it possible to use the errors cached on disk and displaying the source code without loading the file in memory at all?
Maybe that's the primary intent of this PR, not sure I get the whole picture.

---

_Comment by @MichaReiser on 2023-04-08 14:49_

> @MichaReiser Am I understanding correctly that this will also make it possible to use the errors cached on disk and displaying the source code without loading the file in memory at all?

Yes, depending on your definition of *loading the file in memory*. But you could argue that the old implementation did this more efficiently. 

Previously, `Message` only stored the source of the violating line. This has the advantage that a `Message` uses less space than storing the source code of the whole file. But it has a few disadvantages:

* Ruff supports different formats to output the Message: JSON, text, grouped, etc. Only serializing the violating line limits what the formatter can output. E.g. they can't render additional context lines before/after the violation because the source of these lines is unavailable. 
* It limits formatters from rendering an inline diff with the code fix because the fix isn't necessary on the same line. We could include all the lines for every edit, but that means we're already getting closer to just including the source code. 

But our main reason for storing the whole source code on `Message` is that we want to transition to using byte offsets rather than `row/col` locations. But this imposes a problem when rendering `Message`s: We need to compute the line and column information because who wants to see byte offsets in diagnostics? To compute the row/column location we either need an index or... just use the source and recompute the position. That's why `Message` now stores the whole source code and we serialize it as part of the diagnostics. I don't love this... because it means that users we'll have two versions of every file with violations on their disk: The original, and once in our cache (same file as the diagnostics). I considered loading the file instead of duplicating the content into our cache but decided against it because:

* It's more complicated
* It requires reading another file
* We transform Jupyter notebooks before linting (concatenate all cells), meaning we would need to do that too, in a generic way
* Ideally, we would support other resources as source other than files. Maybe a file that you opened in your editor but has no disk representation yet, a file that you provided over STDIN, or the command line arguments... 
* We only store the content for files with violations... there should only be a few files with violations for projects that have adopted ruff. 

So in a sense, yes, this architecture avoids opening the source file and loading its content. But only because it loads the content from the cache ;)

How does elmreview cache diagnostics? Does it re-read the content from the original file when rendering the diagnostic?







---

_Label `internal` added by @MichaReiser on 2023-04-11 07:56_

---

_Merged by @MichaReiser on 2023-04-11 07:57_

---

_Closed by @MichaReiser on 2023-04-11 07:57_

---

_Branch deleted on 2023-04-11 07:57_

---
