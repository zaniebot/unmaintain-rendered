```yaml
number: 3743
title: "[`flake8-pyi`] Implement `PYI012`"
type: pull_request
state: merged
author: JBLDKY
labels:
  - rule
assignees: []
merged: true
base: main
head: pyi012
created_at: 2023-03-26T15:44:53Z
updated_at: 2023-03-27T18:38:31Z
url: https://github.com/astral-sh/ruff/pull/3743
synced_at: 2026-01-12T04:39:45Z
```

# [`flake8-pyi`] Implement `PYI012`

---

_Pull request opened by @JBLDKY on 2023-03-26 15:44_

Looking for some feedback on my implementation of PYI012!

---

_Comment by @github-actions[bot] on 2023-03-26 15:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     13.8±0.04ms     3.0 MB/sec    1.00     13.6±0.05ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.03    502.7±0.98µs     5.9 MB/sec    1.00    489.1±1.10µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.02ms     4.2 MB/sec    1.00      6.0±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.02      7.4±0.09ms     5.5 MB/sec    1.00      7.3±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1670.4±6.37µs    10.0 MB/sec    1.00   1641.0±3.17µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.02    186.7±0.36µs    15.8 MB/sec    1.00    183.5±1.45µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.5±0.01ms     7.3 MB/sec    1.00      3.4±0.01ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     19.6±0.33ms     2.1 MB/sec    1.00     19.4±0.30ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.2±0.10ms     3.2 MB/sec    1.00      5.1±0.12ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.01   653.8±16.97µs     4.5 MB/sec    1.00   647.8±16.05µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.23ms     3.0 MB/sec    1.00      8.6±0.29ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.01     10.6±0.20ms     3.8 MB/sec    1.00     10.5±0.14ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.03ms     7.4 MB/sec    1.02      2.3±0.05ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   263.4±11.22µs    11.2 MB/sec    1.02   269.0±13.82µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.18ms     5.2 MB/sec    1.00      4.9±0.09ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @JonathanPlasse on 2023-03-26 16:12_

LGTM

Do you plan to implement the auto-fix?

---

_Comment by @JBLDKY on 2023-03-26 16:52_

I certainly am planning to implement the auto-fix! 
Thanks for taking a look at the draft

---

_Comment by @charliermarsh on 2023-03-26 17:18_

Take a look at `crates/ruff/src/rules/flake8_pie/rules.rs#no_unnecessary_pass` for an autofix implementation. It's a similar rule (though I'm ok adding this one too).

---

_Comment by @JonathanPlasse on 2023-03-26 21:56_

To test your implementation, you can also run `flake8-pyi` and `ruff` on `typeshed` and see if you have a different number of errors or not.

---

_Comment by @JBLDKY on 2023-03-27 07:13_

Great idea, will do that tonight. Been struggling with the CI Clippy telling me:
```
error[E0599]: no method named `try_amend` found for struct `ruff_diagnostics::Diagnostic` in the current scope
  --> crates/ruff/src/rules/flake8_pyi/rules/pass_in_class_body.rs:39:28
   |
39 |                 diagnostic.try_amend(|| {
   |                            ^^^^^^^^^ method not found in `ruff_diagnostics::Diagnostic`

For more information about this error, try `rustc --explain E0599`.
error: could not compile `ruff` due to previous error
warning: build failed, waiting for other jobs to finish...
Error: Process completed with exit code 101.
```

Clippy complains about no such thing when I run it locally and the pre-commit passes all tests too. 

---

_Comment by @JonathanPlasse on 2023-03-27 07:18_

I had to rebase my branch some time ago to fix a similar error.
`try_amend` was renamed `try_set_fix`.

---

_Comment by @JBLDKY on 2023-03-27 17:10_




> I had to rebase my branch some time ago to fix a similar error. `try_amend` was renamed `try_set_fix`.

Well that's embarrassing, thank you very much. All is good now!

---

_Marked ready for review by @JBLDKY on 2023-03-27 17:10_

---

_Renamed from "Implementation of flake-8 PYI012" to "[`flake8-pyi`] Implement PYI012`" by @charliermarsh on 2023-03-27 18:15_

---

_Renamed from "[`flake8-pyi`] Implement PYI012`" to "[`flake8-pyi`] Implement `PYI012`" by @charliermarsh on 2023-03-27 18:15_

---

_Label `rule` added by @charliermarsh on 2023-03-27 18:15_

---

_Merged by @charliermarsh on 2023-03-27 18:27_

---

_Closed by @charliermarsh on 2023-03-27 18:27_

---

_Branch deleted on 2023-03-27 18:33_

---
