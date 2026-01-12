```yaml
number: 5611
title: Handle io errors gracefully
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: handle_inaccessible_files
created_at: 2023-07-08T13:31:24Z
updated_at: 2023-07-20T09:57:24Z
url: https://github.com/astral-sh/ruff/pull/5611
synced_at: 2026-01-12T03:30:21Z
```

# Handle io errors gracefully

---

_Pull request opened by @konstin on 2023-07-08 13:31_

## Summary

It can happen that we can't read a file (a python file, a jupyter notebook or pyproject.toml), which needs to be handled and handled consistently for all file types. Instead of using `Err` or `error!`, we emit E602 with the io error as message and continue. This PR makes sure we handle all three cases consistently, emit E602.

I'm not convinced that it should be possible to disable io errors, but we now handle the regular case consistently and at least print warning consistently.

I went with `warn!` but i can change them all to `error!`, too.

It also checks the error case when a pyproject.toml is not readable. The error message is not very helpful, but it's now a bit clearer that actually ruff itself failed instead vs this being a diagnostic.

## Examples

This is how an Err of `run` looks now:

![image](https://github.com/astral-sh/ruff/assets/6826232/890f7ab2-2309-4b6f-a4b3-67161947cc83)

With an unreadable file and `IOError` disabled:

![image](https://github.com/astral-sh/ruff/assets/6826232/fd3d6959-fa23-4ddf-b2e5-8d6022df54b1)

(we lint zero files but count files before linting not during so we exit 0)

I'm not sure if it should (or if we should take a different path with manual ExitStatus), but this currently also triggers when `files` is empty:

![image](https://github.com/astral-sh/ruff/assets/6826232/f7ede301-41b5-4743-97fd-49149f750337)

## Test Plan

Unix only: Create a temporary directory with files with permissions `000` (not readable by the owner) and run on that directory. Since this breaks the assumptions of most of the test code (single file, `ruff` instead of `ruff_cli`), the test code is rather cumbersome and looks a bit misplaced; i'm happy about suggestions to fit it in closer with the other tests or streamline it in other ways. I added another test for when the entire directory is not readable.

---

_@konstin reviewed on 2023-07-08 13:32_

---

_Review comment by @konstin on `crates/ruff_cli/tests/integration_test.rs`:311 on 2023-07-08 13:32_

Is configuration discovery the right term?


---

_Renamed from "Handle inaccessible files gracefully" to "Handle io errors gracefully" by @konstin on 2023-07-08 13:32_

---

_Comment by @github-actions[bot] on 2023-07-08 13:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.06ms     4.4 MB/sec    1.02      9.5±0.05ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1857.7±1.63µs     9.0 MB/sec    1.01   1874.9±2.27µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    206.9±0.58µs    14.3 MB/sec    1.01    209.4±0.29µs    14.1 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.4 MB/sec    1.01      4.1±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.07     14.0±0.12ms     2.9 MB/sec    1.00     13.1±0.09ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.4±0.01ms     4.8 MB/sec    1.00      3.3±0.05ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.03    446.1±0.88µs     6.6 MB/sec    1.00    433.9±0.71µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.08      6.4±0.11ms     4.0 MB/sec    1.00      6.0±0.06ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.13      7.5±0.04ms     5.4 MB/sec    1.00      6.6±0.03ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.11   1555.4±4.39µs    10.7 MB/sec    1.00   1407.0±1.46µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.06    165.7±0.22µs    17.8 MB/sec    1.00    155.6±0.36µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.12      3.3±0.01ms     7.7 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.11     16.9±0.73ms     2.4 MB/sec    1.00     15.2±0.60ms     2.7 MB/sec
formatter/numpy/ctypeslib.py               1.07      3.2±0.20ms     5.2 MB/sec    1.00      3.0±0.13ms     5.6 MB/sec
formatter/numpy/globals.py                 1.06   358.9±18.29µs     8.2 MB/sec    1.00   339.2±20.23µs     8.7 MB/sec
formatter/pydantic/types.py                1.04      6.9±0.29ms     3.7 MB/sec    1.00      6.6±0.34ms     3.8 MB/sec
linter/all-rules/large/dataset.py          1.02     22.4±0.79ms  1862.3 KB/sec    1.00     22.0±1.07ms  1893.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      5.7±0.19ms     2.9 MB/sec    1.00      5.5±0.25ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   678.4±38.04µs     4.3 MB/sec    1.02   690.7±32.76µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.6±0.34ms     2.6 MB/sec    1.00      9.6±0.43ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.01     11.8±0.45ms     3.4 MB/sec    1.00     11.7±0.31ms     3.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.4±0.11ms     6.9 MB/sec    1.01      2.4±0.10ms     6.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   283.6±13.14µs    10.4 MB/sec    1.03   292.1±14.79µs    10.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.1±0.19ms     5.0 MB/sec    1.02      5.2±0.18ms     4.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @MichaReiser on 2023-07-12 11:45_

---

_@charliermarsh reviewed on 2023-07-13 00:02_

---

_Review comment by @charliermarsh on `crates/ruff/src/pyproject_toml.rs`:50 on 2023-07-13 00:02_

Can't we not just push inside the `if`, and then return `messages` either way -- since it'd be empty in the `else` case, right?

---

_@charliermarsh reviewed on 2023-07-13 00:02_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/run.rs`:231 on 2023-07-13 00:02_

Should this be `TODO(konsti)`?

---

_@charliermarsh reviewed on 2023-07-13 00:03_

---

_Review comment by @charliermarsh on `crates/ruff/src/settings/mod.rs`:304 on 2023-07-13 00:03_

Why are these no longer `#[cfg(test)]`? Are they used outside of tests?

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:140 on 2023-07-13 00:06_

Are these not already treated as `IOError` via the `IOError` handling in `run.rs`? Like this block:

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

_@charliermarsh reviewed on 2023-07-13 00:06_

---

_@charliermarsh reviewed on 2023-07-13 00:06_

---

_Review comment by @charliermarsh on `crates/ruff_cli/tests/integration_test.rs`:311 on 2023-07-13 00:06_

Yes!

---

_Review comment by @charliermarsh on `crates/ruff/src/settings/mod.rs`:304 on 2023-07-13 00:07_

Oh, perhaps for the configuration tests.

---

_@charliermarsh reviewed on 2023-07-13 00:07_

---

_@charliermarsh reviewed on 2023-07-13 00:07_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/bin/ruff.rs`:61 on 2023-07-13 00:07_

Do you mind including a screenshot of this in the PR summary? Trying to visualize it.

---

_@charliermarsh reviewed on 2023-07-13 00:08_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:140 on 2023-07-13 00:08_

With this change, do we not lose that warning message that appears separately from the diagnostics?

---

_@konstin reviewed on 2023-07-13 07:08_

---

_Review comment by @konstin on `crates/ruff_cli/src/commands/run.rs`:231 on 2023-07-13 07:08_

no it's just a note for when someone wants to add another test case so they don't trip over this

---

_@konstin reviewed on 2023-07-13 07:09_

---

_Review comment by @konstin on `crates/ruff/src/settings/mod.rs`:304 on 2023-07-13 07:09_

they are used across crates now (the test has to live in ruff_cli)) and `cfg(test)` doesn't get passed across crate boundaries

---

_Review comment by @konstin on `crates/ruff_cli/src/bin/ruff.rs`:61 on 2023-07-19 11:17_

done

---

_@konstin reviewed on 2023-07-19 11:17_

---

_Comment by @konstin on 2023-07-19 11:21_

the diagnostics now check whether `IOError` is enabled and if it isn't, print a `warn!` instead (i can change this to an `error!`, not sure which one is better here). I added another test case for an unreadable directory. There are still cases where we didn't lint any file and still exit 0 because `IOError` is disabled and/or we count paths before linting instead of after. I added screenshots to the original change description

---

_Review requested from @charliermarsh by @konstin on 2023-07-19 11:21_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/run.rs`:231 on 2023-07-20 02:04_

I might just suggest removing this comment -- it was confusing enough for me that I had to ask about it :joy:

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/bin/ruff.rs`:55 on 2023-07-20 02:05_

Maybe, for stylistic consistency with the warnings, we do:

```
<bold red>error</><bold>: ruff failed</bold>
  Cause ...
  Cause ...

---

_Review comment by @charliermarsh on `crates/ruff_cli/Cargo.toml`:67 on 2023-07-20 02:05_

Do we need to specify features if they're specified at the workspace level? (I genuinely don't know.)

---

_@charliermarsh approved on 2023-07-20 02:06_

---

_@konstin reviewed on 2023-07-20 09:10_

---

_Review comment by @konstin on `crates/ruff_cli/Cargo.toml`:67 on 2023-07-20 09:10_

surprisingly, yes

---

_@konstin reviewed on 2023-07-20 09:10_

---

_Review comment by @konstin on `crates/ruff_cli/src/bin/ruff.rs`:55 on 2023-07-20 09:10_

done

---

_@konstin reviewed on 2023-07-20 09:10_

---

_Review comment by @konstin on `crates/ruff_cli/src/commands/run.rs`:231 on 2023-07-20 09:10_

fair

---

_Merged by @konstin on 2023-07-20 09:30_

---

_Closed by @konstin on 2023-07-20 09:30_

---

_Branch deleted on 2023-07-20 09:30_

---
