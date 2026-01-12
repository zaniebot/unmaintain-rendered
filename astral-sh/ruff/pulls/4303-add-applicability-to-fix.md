```yaml
number: 4303
title: "Add `Applicability` to `Fix`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: internal/4183
created_at: 2023-05-09T06:13:33Z
updated_at: 2023-05-10T06:42:47Z
url: https://github.com/astral-sh/ruff/pull/4303
synced_at: 2026-01-12T03:56:39Z
```

# Add `Applicability` to `Fix`

---

_Pull request opened by @zanieb on 2023-05-09 06:13_

Adds `Applicability` levels to `Fix` with corresponding factory methods and deprecates the `unspecified` fix methods.

Updates `Diff` output to include "Fix", "Suggested fix", and "Possible fix" depending on the applicability. 

Closes #4183


---

_@zanieb reviewed on 2023-05-09 06:14_

---

_Review comment by @zanieb on `crates/ruff_diagnostics/src/fix.rs`:10 on 2023-05-09 06:14_

I think this may actually be exhaustive with these members; should I drop the `non_exhaustive`?

---

_@zanieb reviewed on 2023-05-09 06:16_

---

_Review comment by @zanieb on `crates/ruff/src/message/diff.rs`:54 on 2023-05-09 06:16_

I think we can change this case to explicitly cover only `Unspecified` if we drop the non-exhaustive from the enum https://github.com/charliermarsh/ruff/pull/4303/files#r1188177338

---

_Review comment by @MichaReiser on `crates/ruff/src/message/diff.rs`:51 on 2023-05-09 06:48_

Nit: What would you think of "Safe fix" (I'm undecided, Fix is short and to the point)?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pie/rules.rs`:140 on 2023-05-09 06:49_

```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pytest_style/rules/assertion.rs`:208 on 2023-05-09 06:49_

```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyupgrade/rules/native_literals.rs`:68 on 2023-05-09 06:52_

The intention looks off, but all good if `rustfmt` is happy.

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:10 on 2023-05-09 06:55_

I think dropping `non_exhaustive` should be fine for now because we don't provide a Rust API, meaning we are in control of all usages and can adopt then when extending the enum. 

---

_Comment by @github-actions[bot] on 2023-05-09 06:56_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.16ms     2.4 MB/sec    1.00     16.8±0.13ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.03ms     4.2 MB/sec    1.00      4.0±0.03ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    490.9±2.78µs     6.0 MB/sec    1.00    491.2±8.12µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.04ms     3.7 MB/sec    1.00      6.9±0.16ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.02      8.3±0.06ms     4.9 MB/sec    1.00      8.1±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1775.6±9.02µs     9.4 MB/sec    1.00  1740.3±12.96µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.02    197.8±2.36µs    14.9 MB/sec    1.00    193.6±4.55µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.7±0.01ms     6.9 MB/sec    1.00      3.6±0.03ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.6±0.03ms     6.2 MB/sec    1.10      7.3±0.05ms     5.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1287.3±3.48µs    12.9 MB/sec    1.09   1397.3±6.70µs    11.9 MB/sec
parser/numpy/globals.py                    1.00    130.4±0.40µs    22.6 MB/sec    1.06    138.8±1.41µs    21.3 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.01ms     9.1 MB/sec    1.09      3.1±0.02ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.2±0.66ms     2.2 MB/sec    1.01     18.5±0.72ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.8±0.26ms     3.5 MB/sec    1.00      4.7±0.30ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   544.0±24.42µs     5.4 MB/sec    1.02   552.6±25.87µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.7±0.37ms     3.3 MB/sec    1.00      7.7±0.35ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      9.3±0.51ms     4.4 MB/sec    1.01      9.4±0.33ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1938.4±78.28µs     8.6 MB/sec    1.02  1980.3±79.16µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   232.7±15.46µs    12.7 MB/sec    1.05   244.3±30.32µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.26ms     6.2 MB/sec    1.03      4.2±0.24ms     6.0 MB/sec
parser/large/dataset.py                    1.00      7.8±0.32ms     5.2 MB/sec    1.02      8.0±0.28ms     5.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1500.9±78.75µs    11.1 MB/sec    1.03  1542.4±71.67µs    10.8 MB/sec
parser/numpy/globals.py                    1.00   150.6±10.53µs    19.6 MB/sec    1.04    156.2±9.08µs    18.9 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.22ms     7.5 MB/sec    1.02      3.4±0.14ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:62 on 2023-05-09 06:57_

I recommend repeating the `automatic` applicability documentation here. Most users will only use the `Fix` API, and explaining what `automatic` safes them a jump in the documentation (or make sure they read it).

```suggestion
    /// Create a new [`Fix`] with [automatic applicability](Applicability::Automatic) from an [`Edit`] element.
```

---

_@MichaReiser approved on 2023-05-09 06:58_

Thank you so much! I hope adding all the `allow(deprecated)` wasn't too much pain?

---

_Label `internal` added by @MichaReiser on 2023-05-09 06:58_

---

_@zanieb reviewed on 2023-05-09 16:03_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pie/rules.rs`:140 on 2023-05-09 16:03_

Thanks! I'll go through the diff once more too and make sure I got all these — I started manually then switched to a regex replace and ended up with some duplicates and weird formatting :)

---

_@zanieb reviewed on 2023-05-09 17:05_

---

_Review comment by @zanieb on `crates/ruff/src/message/diff.rs`:51 on 2023-05-09 17:05_

I'm unsure. It could also be "Automatic fix". I'm leaning towards brevity — it seems like an easy thing to change in the future if users are confused.

---

_@zanieb reviewed on 2023-05-09 17:13_

---

_Review comment by @zanieb on `crates/ruff_diagnostics/src/fix.rs`:62 on 2023-05-09 17:13_

👍 did this for all of the levels

---

_Marked ready for review by @zanieb on 2023-05-09 18:25_

---

_Merged by @MichaReiser on 2023-05-10 06:42_

---

_Closed by @MichaReiser on 2023-05-10 06:42_

---
