```yaml
number: 5578
title: Only run pyproject.toml lint rules when enabled
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/toml
created_at: 2023-07-07T03:06:37Z
updated_at: 2023-07-08T15:05:07Z
url: https://github.com/astral-sh/ruff/pull/5578
synced_at: 2026-01-12T15:55:18Z
```

# Only run pyproject.toml lint rules when enabled

---

_@charliermarsh_

## Summary

I was testing some changes on Airflow, and I realized that we _always_ run the `pyproject.toml` validation rules, even if they're not enabled. This PR gates them behind the appropriate enablement flags.

## Test Plan

- Ran: `cargo run -p ruff_cli -- check ../airflow -n`. Verified that no RUF200 violations were raised.
- Run: `cargo run -p ruff_cli -- check ../airflow -n --select RUF200`. Verified that two RUF200 violations were raised.


---

_Label `bug` added by @charliermarsh on 2023-07-07 03:06_

---

_Review requested from @konstin by @charliermarsh on 2023-07-07 03:06_

---

_Comment by @github-actions[bot] on 2023-07-07 03:15_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.02ms     4.9 MB/sec    1.01      8.5±0.03ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1782.3±2.34µs     9.3 MB/sec    1.02   1813.6±8.21µs     9.2 MB/sec
formatter/numpy/globals.py                 1.00    196.2±2.10µs    15.0 MB/sec    1.01    197.8±0.76µs    14.9 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.4 MB/sec    1.01      4.0±0.00ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.06ms     2.9 MB/sec    1.00     13.9±0.08ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.7±1.15µs     8.1 MB/sec    1.01    368.1±1.67µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.03ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.02ms     5.7 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1465.8±3.87µs    11.4 MB/sec    1.00   1462.8±2.25µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.3±3.59µs    18.6 MB/sec    1.00    158.5±0.98µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.1 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.6±0.53ms     3.5 MB/sec    1.03     11.9±0.71ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.14ms     6.8 MB/sec    1.05      2.6±0.16ms     6.5 MB/sec
formatter/numpy/globals.py                 1.00   289.6±21.86µs    10.2 MB/sec    1.03   298.9±19.42µs     9.9 MB/sec
formatter/pydantic/types.py                1.00      5.7±0.36ms     4.5 MB/sec    1.00      5.7±0.36ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.00     19.5±0.88ms     2.1 MB/sec    1.05     20.4±0.71ms  2037.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.28ms     3.2 MB/sec    1.01      5.2±0.28ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.03   635.6±46.79µs     4.6 MB/sec    1.00   615.1±33.43µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.03      9.1±0.43ms     2.8 MB/sec    1.00      8.8±0.47ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.2±0.50ms     4.0 MB/sec    1.03     10.5±0.44ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.10ms     7.6 MB/sec    1.00      2.2±0.11ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.03   278.9±20.21µs    10.6 MB/sec    1.00   271.6±14.66µs    10.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.24ms     5.5 MB/sec    1.03      4.8±0.23ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-07 05:07_

---

_@konstin requested changes on 2023-07-07 07:52_

I'd implement this differently: Only call `lint_pyproject_toml` if `InvalidPyprojectToml` is enabled, but then raise the io error unconditionally; The io error isn't a real lint but a way to communicate an error (we couldn't read the file, check permissions/filesystem/etc.) using our usual diagnostics system

---

_Comment by @charliermarsh on 2023-07-07 13:57_

@konstin - What about outputting an error to the log, but keeping the conditional? That's what we do for Python files:

```rust
error!(
    "{}{}{} {message}",
    "Failed to lint ".bold(),
    fs::relativize_path(path).bold(),
    ":".bold()
);
let settings = resolver.resolve(path, pyproject_config);
if settings.rules.enabled(Rule::IOError) {
    let file =
        SourceFileBuilder::new(path.to_string_lossy().as_ref(), "").finish();

    Diagnostics::new(
        vec![Message::from_diagnostic(
            Diagnostic::new(IOError { message }, TextRange::default()),
            file,
            TextSize::default(),
        )],
        ImportMap::default(),
    )
} else {
    Diagnostics::default()
}
```

---

_Review requested from @konstin by @charliermarsh on 2023-07-07 13:57_

---

_@konstin approved on 2023-07-08 13:33_

I made io error handling consistent in #5611, which build on top of this PR. I'm not happy with it being possible to disable io errors, but this is unrelated to the pyproject.toml rule.

---

_Merged by @charliermarsh on 2023-07-08 15:05_

---

_Closed by @charliermarsh on 2023-07-08 15:05_

---

_Branch deleted on 2023-07-08 15:05_

---
