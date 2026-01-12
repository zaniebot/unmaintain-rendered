```yaml
number: 4201
title: Print out autofix-broken or non-converging code when debugging
type: pull_request
state: merged
author: akx
labels: []
assignees: []
merged: true
base: main
head: autofix-debug-errors
created_at: 2023-05-03T06:35:20Z
updated_at: 2023-05-03T11:50:04Z
url: https://github.com/astral-sh/ruff/pull/4201
synced_at: 2026-01-12T04:28:19Z
```

# Print out autofix-broken or non-converging code when debugging

---

_Pull request opened by @akx on 2023-05-03 06:35_

Refs https://github.com/charliermarsh/ruff/pull/4196#issuecomment-1532115768

It would be even nicer if we'd also point out where in the source e.g. `f-string: unterminated string at byte offset 205` is, but this is a start :)

The message looks something like

```
$ cargo run --bin=ruff -- --select=FLY --no-cache --diff --fixable=FLY flyntest2.py
   Compiling ruff v0.0.264 (/Users/akx/build/ruff/crates/ruff)
   Compiling ruff_cli v0.0.264 (/Users/akx/build/ruff/crates/ruff_cli)
    Finished dev [unoptimized + debuginfo] target(s) in 23.98s
     Running `target/debug/ruff --select=FLY --no-cache --diff --fixable=FLY flyntest2.py`
debug error: Autofix introduced a syntax error in `flyntest2.py`: f-string: unterminated string at byte offset 205
---
greeting = "Hello"
subject = "World"
msg = greeting + " " + subject
msg2 = f"Finally, {greeting}" + " " + subject
msg3 = f"Finally, {greeting} {subject}"
msg4 = f"Finally, {greeting} {subject}"
msg5 = f"{\"{'Finally'}, \" + f'{greeting}'}{subject}"
x = f"foobar"
y = "foobar"

a = "Hello"
msg1 = f"{a}  World"
msg2 = f"Finally, {a} World"
msg3 = f"1x2x3"
msg4 = "x".join({"4", "5", "yee"})
msg5 = "y".join([1, 2, 3])  # Should be transformed
msg6 = a.join(["1", "2", "3"])  # Should not be transformed (not a static joiner)
msg7 = "a".join(a)  # Should not be transformed (not a static joinee)
msg8 = "a".join([a, a, *a])  # Should not be transformed (not a static length)


---
```
at present, which I suspect might be pretty unwieldy for a longer file. It's a start!

---

_Comment by @github-actions[bot] on 2023-05-03 07:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.0±0.07ms     2.7 MB/sec    1.00     14.8±0.05ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.00ms     4.6 MB/sec    1.00      3.6±0.00ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01    380.0±0.90µs     7.8 MB/sec    1.00    374.8±1.15µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.01ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.6±0.01ms     5.4 MB/sec    1.00      7.5±0.02ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1597.9±6.45µs    10.4 MB/sec    1.00   1584.3±6.12µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    172.5±0.31µs    17.1 MB/sec    1.00    170.8±0.49µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.01ms     7.5 MB/sec    1.00      3.4±0.02ms     7.6 MB/sec
parser/large/dataset.py                    1.00      5.9±0.00ms     6.9 MB/sec    1.11      6.5±0.01ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00   1147.4±0.91µs    14.5 MB/sec    1.09   1254.9±1.62µs    13.3 MB/sec
parser/numpy/globals.py                    1.00    117.2±0.44µs    25.2 MB/sec    1.07    125.4±0.50µs    23.5 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.2 MB/sec    1.09      2.7±0.00ms     9.4 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.06     22.3±0.77ms  1872.1 KB/sec     1.00     21.0±0.96ms  1985.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.08      5.5±0.24ms     3.0 MB/sec     1.00      5.1±0.25ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.10   667.2±46.09µs     4.4 MB/sec     1.00   606.4±43.00µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.05      9.4±0.36ms     2.7 MB/sec     1.00      9.0±0.45ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.05     11.2±0.40ms     3.6 MB/sec     1.00     10.6±0.48ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.3±0.08ms     7.3 MB/sec     1.00      2.3±0.11ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.07   272.8±16.37µs    10.8 MB/sec     1.00   253.9±16.53µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.07      5.1±0.22ms     5.0 MB/sec     1.00      4.7±0.27ms     5.4 MB/sec
parser/large/dataset.py                    1.04      8.8±0.32ms     4.6 MB/sec     1.00      8.4±0.29ms     4.8 MB/sec
parser/numpy/ctypeslib.py                  1.11  1783.2±117.37µs     9.3 MB/sec    1.00  1609.6±59.27µs    10.3 MB/sec
parser/numpy/globals.py                    1.05   172.1±10.38µs    17.1 MB/sec     1.00    164.1±8.08µs    18.0 MB/sec
parser/pydantic/types.py                   1.05      3.9±0.26ms     6.6 MB/sec     1.00      3.7±0.16ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/linter.rs`:523 on 2023-05-03 09:54_

I recommend you to define the function once for `debug_assertions` and once for release to avoid the clippy errors:

```suggestion

#[cfg(debug_assertions)]
#[allow(clippy::print_stderr)]
fn report_failed_to_converge_error(path: &Path, transformed: &str) {
    eprintln!(
        "{}: Failed to converge after {} iterations in `{}`:---\n{}\n---",
        "debug error".red().bold(),
        MAX_ITERATIONS,
        fs::relativize_path(path),
        transformed,
    );
}

#[cfg(not(debug_assertions))]
fn report_failed_to_converge_error(path: &Path, _transformed: &str) {
  eprintln!(
      r#"
{}: Failed to converge after {} iterations.

This indicates a bug in `{}`. If you could open an issue at:

{}/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `{}`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!
"#,
      "error".red().bold(),
      MAX_ITERATIONS,
      env!("CARGO_PKG_NAME"),
      env!("CARGO_PKG_REPOSITORY"),
      fs::relativize_path(path),
  );
}
```

---

_Review comment by @MichaReiser on `crates/ruff/src/linter.rs`:530 on 2023-05-03 09:57_

Generating a nice readable error is possible as you can see in, but it's a bit cumbersome to do so at the moment.

https://github.com/charliermarsh/ruff/blob/719395f487018783b20c82198f61dc0f2a229bc6/crates/ruff/src/test.rs#L116-L119

Up to you if you want to give it a try

---

_@MichaReiser requested changes on 2023-05-03 09:57_

Looks good to me and thanks for contributing. 

Please take a look at the clippy failures and could you add a code snipped showing the Ruff output when a fix introduces a syntax error for a debug and a release build?

---

_@akx reviewed on 2023-05-03 10:02_

---

_Review comment by @akx on `crates/ruff/src/linter.rs`:523 on 2023-05-03 10:02_

The clippy errors were for a previous version of this PR, CI seems green now.

**EDIT:** right, there are warnings when building for release, never mind...


---

_@akx reviewed on 2023-05-03 10:38_

---

_Review comment by @akx on `crates/ruff/src/linter.rs`:530 on 2023-05-03 10:38_

@MichaReiser Maybe once that code is in `main` :)

---

_Review requested from @MichaReiser by @akx on 2023-05-03 10:39_

---

_@MichaReiser approved on 2023-05-03 11:49_

Thank you

---

_Merged by @MichaReiser on 2023-05-03 11:50_

---

_Closed by @MichaReiser on 2023-05-03 11:50_

---
