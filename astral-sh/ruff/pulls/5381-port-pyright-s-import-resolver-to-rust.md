```yaml
number: 5381
title: "Port Pyright's import resolver to Rust"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/import-resolver
created_at: 2023-06-27T04:18:17Z
updated_at: 2023-06-28T05:22:54Z
url: https://github.com/astral-sh/ruff/pull/5381
synced_at: 2026-01-12T15:55:18Z
```

# Port Pyright's import resolver to Rust

---

_@charliermarsh_

## Summary

This PR contains the first step towards enabling robust first-party, third-party, and standard library import resolution in Ruff (including support for `typeshed`, stub files, native modules, etc.) by porting Pyright's import resolver to Rust.

The strategy taken here was to start with a more-or-less direct port of the Pyright's TypeScript resolver. The code is intentionally similar, and the test suite is effectively a superset of Pyright's test suite for its own resolver. Due to the nature of the port, the code is very, very non-idiomatic for Rust. The code is also entirely unused outside of the test suite, and no effort has been made to integrate it with the rest of the codebase.

Future work will include:

- Refactoring the code (now that it works) to match Rust and Ruff idioms.
- Further testing, in practice, to ensure that the resolver can resolve imports in a complex project, when provided with a virtual environment path.
- Caching, to minimize filesystem lookups and redundant resolutions.
- Integration into Ruff itself (use Ruff's existing settings, find rules that can make use of robust resolution, etc.)

---

_Review requested from @MichaReiser by @charliermarsh on 2023-06-27 04:18_

---

_Review requested from @konstin by @charliermarsh on 2023-06-27 04:18_

---

_Comment by @charliermarsh on 2023-06-27 04:19_

Candidly, I don't know that it's worth doing a close review of the code in its current state. I want to check this in, since it closely mirrors the source implementation -- but I plan to refactor large parts of it, and I suspect the reviews on those refactors will be more informative, accessible, and worthwhile.

---

_@charliermarsh reviewed on 2023-06-27 04:20_

---

_Review comment by @charliermarsh on `crates/ruff_python_resolver/src/config.rs`:7 on 2023-06-27 04:20_

We won't need all of these, I don't think.

---

_Comment by @Boshen on 2023-06-27 04:36_

I worked on the Rust version of the node.js resolver. An abstraction you want to add early is the concept of "normalized" or "canonicalized" path, you won't be able to distinguish these to the path read from the import statements very soon ...

Another pedantic thing is to use `Box<Path>` instead of `PathBuf`.

---

_Comment by @github-actions[bot] on 2023-06-27 04:45_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.9±0.05ms     5.2 MB/sec    1.00      7.8±0.02ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1697.7±2.55µs     9.8 MB/sec    1.00   1693.1±2.47µs     9.8 MB/sec
formatter/numpy/globals.py                 1.00    191.1±0.30µs    15.4 MB/sec    1.01    192.3±1.45µs    15.3 MB/sec
formatter/pydantic/types.py                1.00      3.7±0.01ms     6.8 MB/sec    1.00      3.7±0.01ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.01     13.3±0.11ms     3.1 MB/sec    1.00     13.2±0.08ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.01      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    425.5±1.31µs     6.9 MB/sec    1.01    430.2±0.77µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.06ms     4.3 MB/sec    1.00      5.8±0.03ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.02ms     6.1 MB/sec    1.00      6.6±0.03ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1461.1±1.88µs    11.4 MB/sec    1.01   1478.9±1.85µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.7±1.61µs    17.8 MB/sec    1.01    167.5±0.22µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.01      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.08ms     4.4 MB/sec    1.01      9.4±0.13ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1908.3±16.01µs     8.7 MB/sec    1.01  1920.9±16.43µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    209.4±1.85µs    14.1 MB/sec    1.00    210.0±3.77µs    14.1 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.03ms     5.8 MB/sec    1.01      4.4±0.06ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.14ms     2.6 MB/sec    1.00     15.5±0.15ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.06ms     4.0 MB/sec    1.00      4.1±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    426.8±5.92µs     6.9 MB/sec    1.02    433.2±6.35µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.09ms     3.7 MB/sec    1.00      6.9±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.06ms     5.1 MB/sec    1.02      8.2±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1670.0±14.27µs    10.0 MB/sec    1.03  1719.1±14.87µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.9±1.42µs    16.4 MB/sec    1.02    184.0±2.21µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.0 MB/sec    1.02      3.7±0.04ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff_python_resolver/Cargo.toml`:17 on 2023-06-27 07:45_

not specific to this PR, but i've been thinking about switching to tracing instead, which has the same logging macros but some extra goodies such as spans and tracing_subscriber. I haven't checked if that still works nicely with fern.

---

_Review comment by @konstin on `crates/ruff_python_resolver/src/host.rs`:42 on 2023-06-27 07:49_

this is also a todo

---

_Review comment by @konstin on `crates/ruff_python_resolver/src/implicit_imports.rs`:14 on 2023-06-27 07:52_

What's an implicit import of a native module? https://stackoverflow.com/questions/48716943/what-is-python-implicit-relative-import says implicit imports have been removed in python 3. (I do see how type stubs can be described as an implicit import)

---

_Review comment by @konstin on `crates/ruff_python_resolver/src/implicit_imports.rs`:31 on 2023-06-27 07:53_

We also have to think about error handling when we start having dependencies on io, e.g. this would be weird if that was a permission error. Which is also a good opportunity to promote https://docs.rs/fs-err/latest/fs_err/, it made a big difference in maturin

---

_Review comment by @konstin on `crates/ruff_python_resolver/src/implicit_imports.rs`:66 on 2023-06-27 07:57_

This could be `foo.so`, `foo.abi3.so` or `foo.cpython-38-x86_64-linux-gnu.so`, but i haven't seen `foo.py.so`

---

_Review comment by @konstin on `crates/ruff_python_resolver/src/import_result.rs`:46 on 2023-06-27 08:02_

are those the files we need to import before we import out actual target file?

---

_Review comment by @konstin on `crates/ruff_python_resolver/src/lib.rs`:17 on 2023-06-27 08:03_

on windows it's `Lib`

---

_Review comment by @konstin on `crates/ruff_python_resolver/src/search.rs`:29 on 2023-06-27 08:24_

shouldn't `python{major}.{minor}` (unix) or `Python` (iirc windows) we in between the two? i've never seen the version without it

---

_Review comment by @konstin on `crates/ruff_python_resolver/src/search.rs`:53 on 2023-06-27 08:26_

Strange they do it that way, if know the python version we can just compute the path

---

_Review comment by @konstin on `crates/ruff_python_resolver/src/search.rs`:95 on 2023-06-27 08:28_

pth files are such a hack because the canonical way to handle them is to execute arbitrary code every time before any user code

---

_Review comment by @konstin on `crates/ruff_python_resolver/src/search.rs`:144 on 2023-06-27 08:29_

here i'd also just check the current platform

---

_@konstin approved on 2023-06-27 08:31_

Read through it once, didn't flag any rust idioms

---

_@charliermarsh reviewed on 2023-06-27 16:01_

---

_Review comment by @charliermarsh on `crates/ruff_python_resolver/Cargo.toml`:17 on 2023-06-27 16:01_

I think they used tracing at Rome, @MichaReiser has mentioned this once before... No opinion from me! (I also don't feel strongly about fern or chrono or anything else.)

---

_@charliermarsh reviewed on 2023-06-27 16:01_

---

_Review comment by @charliermarsh on `crates/ruff_python_resolver/src/host.rs`:42 on 2023-06-27 16:01_

I think this is fine because it's just for testing.

---

_Review comment by @charliermarsh on `crates/ruff_python_resolver/src/import_result.rs`:46 on 2023-06-27 16:04_

These are all the files imported when "importing" the module. E.g., if you `import foo.bar`, it'll include the `__init__.py` file in `./foo`, and `./foo/bar.py`.

---

_@charliermarsh reviewed on 2023-06-27 16:04_

---

_@charliermarsh reviewed on 2023-06-27 16:04_

---

_Review comment by @charliermarsh on `crates/ruff_python_resolver/src/import_result.rs`:46 on 2023-06-27 16:04_

Will add some examples.

---

_@charliermarsh reviewed on 2023-06-27 16:04_

---

_Review comment by @charliermarsh on `crates/ruff_python_resolver/src/implicit_imports.rs`:66 on 2023-06-27 16:04_

Good call.

---

_@charliermarsh reviewed on 2023-06-27 16:06_

---

_Review comment by @charliermarsh on `crates/ruff_python_resolver/src/search.rs`:29 on 2023-06-27 16:06_

Can you check the structure on Windows for me?

---

_Merged by @charliermarsh on 2023-06-27 16:15_

---

_Closed by @charliermarsh on 2023-06-27 16:15_

---

_Branch deleted on 2023-06-27 16:15_

---

_@konstin reviewed on 2023-06-28 05:22_

---

_Review comment by @konstin on `crates/ruff_python_resolver/src/search.rs`:29 on 2023-06-28 05:22_

sorry windows actually uses `Lib/site-packages`

![image](https://github.com/astral-sh/ruff/assets/6826232/eba30280-1aa2-40b1-a3bb-17dba7b9b75b)


---
