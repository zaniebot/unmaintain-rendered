```yaml
number: 6629
title: Avoid cloning source code multiple times
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - performance
assignees: []
merged: true
base: main
head: charlie/cow
created_at: 2023-08-16T19:25:52Z
updated_at: 2023-08-18T13:32:21Z
url: https://github.com/astral-sh/ruff/pull/6629
synced_at: 2026-01-12T02:52:04Z
```

# Avoid cloning source code multiple times

---

_Pull request opened by @charliermarsh on 2023-08-16 19:25_

## Summary

In working on https://github.com/astral-sh/ruff/pull/6628, I noticed that we clone the source code contents, potentially multiple times, prior to linting. The issue is that `SourceKind::Python` takes a `String`, so we first have to provide it with a `String`. In the stdin case, that means cloning. However, on top of this, we then have to clone `source_kind.contents()` because `SourceKind` gets mutated. So for stdin, we end up cloning twice. For non-stdin, we end up cloning once, but unnecessarily (since the _contents_ don't get mutated, only the kind).

This PR removes the `String` from `source_kind`, instead requiring that we parse it out elsewhere. It reduces the number of clones down to 1 for Jupyter Notebooks, and zero otherwise.


---

_Label `internal` added by @charliermarsh on 2023-08-16 19:26_

---

_Label `performance` added by @charliermarsh on 2023-08-16 19:26_

---

_Marked ready for review by @charliermarsh on 2023-08-16 19:26_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-08-16 19:26_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:525 on 2023-08-16 19:49_

I find `Sources` a confusing name, because it only contains a single source file. 

Could we maybe marry this with our `SourceFile` implementation that also gives us cheap cloning? 

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:560 on 2023-08-16 19:51_

This is unrelated to this PR, is it possible to have a Jupyter notebook with a Pyi source type? I'm asking because I wonder if this should be a single union instead.

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:543 on 2023-08-16 19:51_

Nit: `Sources::try_fom_path` or `Sources::read_file`

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:569 on 2023-08-16 19:51_

Nit: Sources::from_contents`

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:604 on 2023-08-16 19:55_

Nit: `Diagnostics:from_io_error`

---

_Comment by @github-actions[bot] on 2023-08-16 19:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.0±0.16ms    10.3 MB/sec    1.04      4.1±0.20ms     9.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   818.3±39.71µs    20.3 MB/sec    1.00   814.7±38.52µs    20.4 MB/sec
formatter/numpy/globals.py                 1.01     85.1±5.47µs    34.7 MB/sec    1.00     84.4±4.46µs    35.0 MB/sec
formatter/pydantic/types.py                1.00  1632.6±80.37µs    15.6 MB/sec    1.05  1716.3±137.12µs    14.9 MB/sec
linter/all-rules/large/dataset.py          1.00     12.9±0.52ms     3.1 MB/sec    1.02     13.2±0.53ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.11ms     4.8 MB/sec    1.00      3.4±0.12ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   497.4±21.70µs     5.9 MB/sec    1.03   514.3±20.54µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.31ms     3.7 MB/sec    1.00      6.8±0.26ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.22ms     6.0 MB/sec    1.01      6.9±0.26ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1481.6±59.80µs    11.2 MB/sec    1.00  1461.6±55.76µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   185.1±10.97µs    15.9 MB/sec    1.02    188.6±8.34µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.14ms     8.4 MB/sec    1.00      3.1±0.11ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.1±0.05ms     9.9 MB/sec    1.01      4.2±0.10ms     9.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   820.6±17.11µs    20.3 MB/sec    1.00   819.4±11.63µs    20.3 MB/sec
formatter/numpy/globals.py                 1.00     85.0±4.03µs    34.7 MB/sec    1.00     84.7±2.46µs    34.8 MB/sec
formatter/pydantic/types.py                1.00  1658.8±26.36µs    15.4 MB/sec    1.01  1680.5±28.82µs    15.2 MB/sec
linter/all-rules/large/dataset.py          1.01     12.8±0.23ms     3.2 MB/sec    1.00     12.7±0.11ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.07ms     4.8 MB/sec    1.00      3.5±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   443.4±10.99µs     6.7 MB/sec    1.00    441.6±7.35µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.09ms     3.9 MB/sec    1.00      6.6±0.10ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.06ms     5.8 MB/sec    1.00      7.0±0.06ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1491.0±18.54µs    11.2 MB/sec    1.00  1481.3±25.24µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    177.3±4.66µs    16.6 MB/sec    1.00    175.9±6.45µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.2 MB/sec    1.00      3.1±0.05ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:589 on 2023-08-16 20:00_

Could we instead change `SourceKind` to: 

```
enum SourceKind<'a> {
	Python(&'a str),
	Notebook(Notebook)
}

impl SourceKind<'_> {
	fn source_code(&self) -> &str {
		match self {
			Self::Python(source) => source,
			Self::Notebook(notebook) => notebook.contents()
		}
}
```

---

_@zanieb approved on 2023-08-16 20:03_

---

_@MichaReiser reviewed on 2023-08-16 20:05_

Nice, this should reduce memory usage quiet a bit and may even improve the cpython benchmark. 

The new solution still feels a bit awkward to me. Maybe it is because I don't understand the separation between `Sources`, `SourceType`, and `SourceKind` well enough. Maybe we don't have these abstractions right yet which makes this more complicated than it needs to be? Could it also help to avoid storing the `Notebook` contents twice?

---

_@MichaReiser reviewed on 2023-08-16 20:06_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:589 on 2023-08-16 20:06_

I'm using `source_code`here (or `source`) because I find `contents` a too generic term (I can't even tell from its name if it is a string)

---

_@charliermarsh reviewed on 2023-08-16 20:09_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:589 on 2023-08-16 20:09_

This was my instinct too. I tried it but it didn’t work, because the Diagnostics struct includes the SourceKind so that it can reconstruct the cell ranges when it reports diagnostics at the end. So we’d have to add lifetimes to Diagnostics and then keep all the source code in memory for the life of the program.

---

_Comment by @charliermarsh on 2023-08-16 20:10_

It feels awkward to me too, but I didn’t want to do anything more invasive. I want to see if we can remove the contents from Notebook, perhaps, which would make the responsibilities clearer.

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:560 on 2023-08-16 20:10_

It’s not possible, no

---

_@charliermarsh reviewed on 2023-08-16 20:10_

---

_@MichaReiser reviewed on 2023-08-16 20:23_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:589 on 2023-08-16 20:23_

Hmm I see. I have to take a closer look at this tomorrow. I wonder if `SourceKind` (or something similar) should be a trait and / or if we can structure these types differently to also avoid the overlap between `SourceKind` and `SourceType`

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:589 on 2023-08-16 20:26_

Diagnostics only needs access to the `index` though, not the file contents, which may help.

---

_@charliermarsh reviewed on 2023-08-16 20:26_

---

_@MichaReiser reviewed on 2023-08-16 20:33_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:589 on 2023-08-16 20:33_

Yeah, it's another source mapping (the reverse after applying fixes). It would be nice to have a more formal concept for that rather than special casing jupyter notebooks in the `Emitter`. Because we'll have the same problem when linting markdown files: We need to map back the relative code block indices to the absolute file indices, potentially customizing the message.

---

_Comment by @charliermarsh on 2023-08-16 20:40_

@MichaReiser - I made some improvements based on your suggestions but LMK how you want to proceed (e.g., whether you want to spend time on this, or want me to do so, or want to merge and revisit later).

---

_Comment by @MichaReiser on 2023-08-17 07:51_

Current dependencies on/for this PR:
* main
  * **PR #6629** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6629" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #6640** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6640" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/6629?utm_source=stack-comment).

---

_@MichaReiser approved on 2023-08-17 07:52_

Thanks. I like the improvements and the improved naming. 

We should follow up on notebooks. I think it would be nice if `SourceKind` could store the source code as well, to avoid passing two arguments everywhere. However, I didn't manage to do that because of some lifetime issues when fixing notebooks (and updating notebook indices)

---

_@dhruvmanila approved on 2023-08-17 19:40_

Looks good! Thanks for doing this :)

---

_Merged by @charliermarsh on 2023-08-18 13:32_

---

_Closed by @charliermarsh on 2023-08-18 13:32_

---

_Branch deleted on 2023-08-18 13:32_

---
