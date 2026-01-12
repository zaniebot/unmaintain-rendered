```yaml
number: 5534
title: "Run `cargo update`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/update
created_at: 2023-07-05T16:09:34Z
updated_at: 2023-07-05T16:43:42Z
url: https://github.com/astral-sh/ruff/pull/5534
synced_at: 2026-01-12T15:55:18Z
```

# Run `cargo update`

---

_@charliermarsh_

```console
❯ cargo update
    Updating crates.io index
    Updating git repository `https://github.com/charliermarsh/LibCST`
    Updating git repository `https://github.com/astral-sh/RustPython-Parser.git`
    Updating git repository `https://github.com/youknowone/unicode_names2.git`
    Updating bitflags v2.3.2 -> v2.3.3
    Updating bstr v1.5.0 -> v1.6.0
    Updating clap v4.3.8 -> v4.3.11
    Updating clap_builder v4.3.8 -> v4.3.11
    Updating clap_complete v4.3.1 -> v4.3.2
    Updating colored v2.0.0 -> v2.0.4
    Removing hermit-abi v0.2.6
    Removing hermit-abi v0.3.1
      Adding hermit-abi v0.3.2
    Updating is-terminal v0.4.7 -> v0.4.8
    Updating itoa v1.0.6 -> v1.0.8
      Adding linux-raw-sys v0.4.3
    Updating num_cpus v1.15.0 -> v1.16.0
    Updating paste v1.0.12 -> v1.0.13
    Updating pin-project-lite v0.2.9 -> v0.2.10
    Updating quote v1.0.28 -> v1.0.29
    Updating regex v1.8.4 -> v1.9.0
    Updating regex-automata v0.1.10 -> v0.3.0
    Updating regex-syntax v0.7.2 -> v0.7.3
    Removing rustix v0.37.20
      Adding rustix v0.37.23
      Adding rustix v0.38.3
    Updating rustversion v1.0.12 -> v1.0.13
    Updating ryu v1.0.13 -> v1.0.14
    Updating serde v1.0.164 -> v1.0.166
    Updating serde_derive v1.0.164 -> v1.0.166
    Updating serde_json v1.0.99 -> v1.0.100
    Updating syn v2.0.22 -> v2.0.23
    Updating thiserror v1.0.40 -> v1.0.41
    Updating thiserror-impl v1.0.40 -> v1.0.41
    Updating unicode-ident v1.0.9 -> v1.0.10
    Updating uuid v1.3.4 -> v1.4.0
    Updating windows-targets v0.48.0 -> v0.48.1
```

---

_Comment by @charliermarsh on 2023-07-05 16:18_

Also able to remove `atty` for some reason:

```
unused dependencies:
`ruff_cli v0.0.277 (/home/runner/work/ruff/ruff/crates/ruff_cli)`
└─── dependencies
     └─── "atty"
Note: These dependencies might be used by other targets.
      To find dependencies that are not used by any target, enable `--all-targets`.
Note: They might be false-positive.
      For example, `cargo-udeps` cannot detect usage of crates that are only used in doc-tests.
      To ignore some dependencies, write `package.metadata.cargo-udeps.ignore` in Cargo.toml.
```

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-05 16:18_

---

_@MichaReiser approved on 2023-07-05 16:33_

---

_Merged by @charliermarsh on 2023-07-05 16:34_

---

_Closed by @charliermarsh on 2023-07-05 16:34_

---

_Branch deleted on 2023-07-05 16:34_

---

_Comment by @github-actions[bot] on 2023-07-05 16:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04      8.9±0.31ms     4.6 MB/sec    1.00      8.6±0.46ms     4.7 MB/sec
formatter/numpy/ctypeslib.py               1.03  1946.9±79.01µs     8.6 MB/sec    1.00  1886.3±100.90µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00   207.3±10.78µs    14.2 MB/sec    1.09   226.0±10.66µs    13.1 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.19ms     6.0 MB/sec    1.00      4.2±0.15ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.06     15.4±0.44ms     2.6 MB/sec    1.00     14.5±0.59ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.7±0.15ms     4.5 MB/sec    1.00      3.7±0.18ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.01   474.6±17.55µs     6.2 MB/sec    1.00   469.5±22.69µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.29ms     4.0 MB/sec    1.03      6.5±0.28ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.07      7.9±0.18ms     5.2 MB/sec    1.00      7.3±0.24ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07   1715.6±1.76µs     9.7 MB/sec    1.00  1609.6±76.29µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    179.7±8.76µs    16.4 MB/sec    1.00    178.5±9.48µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.04      3.4±0.18ms     7.4 MB/sec    1.00      3.3±0.15ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.09     13.5±0.39ms     3.0 MB/sec    1.00     12.4±0.36ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.05      2.8±0.11ms     5.9 MB/sec    1.00      2.7±0.18ms     6.2 MB/sec
formatter/numpy/globals.py                 1.07   310.1±18.06µs     9.5 MB/sec    1.00   288.7±14.08µs    10.2 MB/sec
formatter/pydantic/types.py                1.08      6.2±0.28ms     4.1 MB/sec    1.00      5.8±0.20ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.00     19.4±0.44ms     2.1 MB/sec    1.00     19.5±0.59ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.16ms     3.3 MB/sec    1.01      5.2±0.19ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   631.3±22.80µs     4.7 MB/sec    1.00   630.7±21.70µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.25ms     2.9 MB/sec    1.00      8.7±0.33ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.1±0.27ms     4.0 MB/sec    1.00     10.1±0.25ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.07ms     7.7 MB/sec    1.00      2.1±0.07ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.02   251.7±10.22µs    11.7 MB/sec    1.00    245.6±9.03µs    12.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.5±0.14ms     5.6 MB/sec    1.00      4.5±0.15ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
