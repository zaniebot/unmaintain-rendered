```yaml
number: 3914
title: "Allow users to extend the set of included files via `include`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/include
created_at: 2023-04-08T04:14:44Z
updated_at: 2023-04-12T03:39:45Z
url: https://github.com/astral-sh/ruff/pull/3914
synced_at: 2026-01-12T15:55:14Z
```

# Allow users to extend the set of included files via `include`

---

_@charliermarsh_

## Summary

This PR adds an `include` mechanism to Ruff's configuration, which mirrors `exclude` (and `extend_exclude`), but is applied _after_ exclusions, and allows users to include files beyond `*.pyi` and `*.py` (the default) when running Ruff.

For example, if users want to lint `.pyw` files in addition to `.pyi` and `py` files, they could add:

```toml
[tool.ruff]
extend-include = ["*.pyw"]
```

Closes #3410.

Closes #3783.

Closes #2192 (by providing a workaround).


---

_Comment by @charliermarsh on 2023-04-08 04:18_

This introduces a small performance regression (from ~247ms to ~251ms on CPython).


---

_Comment by @github-actions[bot] on 2023-04-08 04:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     15.3±0.71ms     2.7 MB/sec    1.00     15.0±0.79ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.9±0.21ms     4.3 MB/sec    1.00      3.7±0.21ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.08   526.0±15.70µs     5.6 MB/sec    1.00   486.2±31.93µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.04      6.7±0.38ms     3.8 MB/sec    1.00      6.5±0.42ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.01      8.0±0.36ms     5.1 MB/sec    1.00      7.9±0.38ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1871.4±78.63µs     8.9 MB/sec    1.00  1812.1±80.79µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   194.7±12.37µs    15.2 MB/sec    1.04   201.6±10.75µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.06      3.8±0.18ms     6.7 MB/sec    1.00      3.6±0.23ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.3±0.36ms     2.5 MB/sec    1.00     16.2±0.16ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.0 MB/sec    1.01      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    491.3±6.23µs     6.0 MB/sec    1.02    498.7±8.13µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.15ms     3.7 MB/sec    1.00      6.9±0.10ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.07ms     5.0 MB/sec    1.03      8.4±0.11ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1777.4±30.43µs     9.4 MB/sec    1.04  1846.4±30.48µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.7±4.41µs    15.3 MB/sec    1.02    197.5±5.24µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     6.8 MB/sec    1.02      3.8±0.08ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-04-08 15:14_

---

_Comment by @charliermarsh on 2023-04-08 15:15_

I think we can improve performance by tracking whether _any_ settings have custom inclusions or exclusions, and avoiding the settings resolution altogether if not. But, that would be better as a separate PR anyway.

---

_Review comment by @MichaReiser on `crates/ruff/src/resolver.rs`:331 on 2023-04-11 06:21_

Nit: I wonder if it would improve performance if we merge `extend_include` and `include`. 

---

_Comment by @MichaReiser on 2023-04-11 06:24_

I think this is a good change to unlock users, even though I would have preferred a new settings schema first, but this is taking too long.

> This PR adds an include mechanism to Ruff's configuration, which mirrors exclude (and extend_exclude), but is applied after exclusions, and allows users to include files beyond *.pyi and *.py (the default) when running Ruff.

I would have expected the opposite: `include`s define the initial file set and `exclude` filter the files. What's your motivation for choosing this precedence? 

Do you know how flake8 supports these use cases? I wasn't able to find an `include` option.

---

_@MichaReiser approved on 2023-04-11 06:24_

---

_@charliermarsh reviewed on 2023-04-12 03:30_

---

_Review comment by @charliermarsh on `crates/ruff/src/resolver.rs`:331 on 2023-04-12 03:30_

It's nice to have the separate debug "reason" but I agree, in theory at least it would be faster. In practice (and in my benchmarks), `extend_include` is almost always empty.

---

_@charliermarsh reviewed on 2023-04-12 03:30_

---

_Review comment by @charliermarsh on `crates/ruff/src/resolver.rs`:327 on 2023-04-12 03:30_

I think the real performance penalty is having to resolve settings for every file.

---

_Comment by @charliermarsh on 2023-04-12 03:32_

> I would have expected the opposite: includes define the initial file set and exclude filter the files. What's your motivation for choosing this precedence?

> Do you know how flake8 supports these use cases? I wasn't able to find an include option.

Good question -- the precedence here is from Black, here's the documentation:

```
  --include TEXT                  A regular expression that matches files and
                                  directories that should be included on
                                  recursive searches. An empty value means all
                                  files are included regardless of the name.
                                  Use forward slashes for directories on all
                                  platforms (Windows, too). Exclusions are
                                  calculated first, inclusions later.
                                  [default: \.pyi?$]
  --exclude TEXT                  A regular expression that matches files and
                                  directories that should be excluded on
                                  recursive searches. An empty value means no
                                  paths are excluded. Use forward slashes for
                                  directories on all platforms (Windows, too).
                                  Exclusions are calculated first, inclusions
                                  later. [default: /(\.direnv|\.eggs|\.git|\.h
                                  g|\.mypy_cache|\.nox|\.tox|\.venv|venv|\.svn
                                  |\.ipynb_checkpoints|_build|buck-
```

---

_Comment by @charliermarsh on 2023-04-12 03:33_

(Related to the above docs: I don't really know what it would mean to apply an inclusion to a _directory_ in this context, since we traverse over all directories by default and exclusions are applied first (so you can never include a subdirectory within an excluded parent). So I'm only testing inclusions against _files_.)

---

_Comment by @charliermarsh on 2023-04-12 03:38_

Relevant stuff from Black:

https://github.com/psf/black/pull/281
https://github.com/psf/black/issues/270

---

_Comment by @charliermarsh on 2023-04-12 03:39_

AFAICT, they also only apply inclusion checks to individual files.

---

_Merged by @charliermarsh on 2023-04-12 03:39_

---

_Closed by @charliermarsh on 2023-04-12 03:39_

---

_Branch deleted on 2023-04-12 03:39_

---
