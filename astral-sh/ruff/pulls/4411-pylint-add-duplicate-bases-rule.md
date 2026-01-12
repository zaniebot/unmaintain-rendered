```yaml
number: 4411
title: "[`pylint`] Add `duplicate-bases` rule"
type: pull_request
state: merged
author: alonme
labels:
  - rule
assignees: []
merged: true
base: main
head: pylint-duplicate-bases
created_at: 2023-05-13T12:51:28Z
updated_at: 2023-05-18T21:20:41Z
url: https://github.com/astral-sh/ruff/pull/4411
synced_at: 2026-01-12T15:55:15Z
```

# [`pylint`] Add `duplicate-bases` rule

---

_@alonme_

This is part of https://github.com/charliermarsh/ruff/issues/970

---

_Comment by @github-actions[bot] on 2023-05-13 13:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.35ms     2.9 MB/sec    1.01     14.4±0.39ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.09ms     4.8 MB/sec    1.07      3.8±0.15ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   433.5±15.38µs     6.8 MB/sec    1.07   462.1±15.43µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.20ms     4.2 MB/sec    1.03      6.2±0.20ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.19ms     5.8 MB/sec    1.06      7.5±0.14ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1554.6±67.32µs    10.7 MB/sec    1.08  1682.3±41.77µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.4±7.53µs    17.6 MB/sec    1.08    180.0±7.30µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.10ms     8.1 MB/sec    1.07      3.4±0.13ms     7.5 MB/sec
parser/large/dataset.py                    1.01      5.6±0.15ms     7.3 MB/sec    1.00      5.5±0.13ms     7.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1076.4±39.95µs    15.5 MB/sec    1.02  1101.6±54.51µs    15.1 MB/sec
parser/numpy/globals.py                    1.00    109.8±4.85µs    26.9 MB/sec    1.01    111.4±4.48µs    26.5 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.07ms    10.9 MB/sec    1.03      2.4±0.10ms    10.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.2±0.11ms     2.5 MB/sec    1.01     16.3±0.14ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.04ms     4.0 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    492.6±9.25µs     6.0 MB/sec    1.00    494.7±9.19µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.04ms     3.7 MB/sec    1.01      6.9±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.04ms     5.1 MB/sec    1.01      8.1±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1703.4±18.46µs     9.8 MB/sec    1.01  1714.4±14.96µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.7±3.05µs    15.4 MB/sec    1.02    195.3±5.56µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.1 MB/sec    1.01      3.7±0.03ms     7.0 MB/sec
parser/large/dataset.py                    1.02      6.7±0.08ms     6.0 MB/sec    1.00      6.6±0.04ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.01  1279.3±11.70µs    13.0 MB/sec    1.00  1263.7±11.92µs    13.2 MB/sec
parser/numpy/globals.py                    1.01    131.3±2.23µs    22.5 MB/sec    1.00    130.0±1.87µs    22.7 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.03ms     8.9 MB/sec    1.00      2.8±0.03ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @charliermarsh on 2023-05-13 13:33_

---

_Label `rule` added by @charliermarsh on 2023-05-13 14:16_

---

_Comment by @charliermarsh on 2023-05-13 14:17_

If you're interested, we can autofix this. Take a look at `crates/ruff/src/rules/pyupgrade/rules/useless_object_inheritance.rs` which is a very similar fix (removes `object` from `class Foo(object)`).

---

_Renamed from "Add pylint duplicate-bases rule" to "[`pylint`] Add `duplicate-bases` rule" by @charliermarsh on 2023-05-13 14:17_

---

_Merged by @charliermarsh on 2023-05-13 14:28_

---

_Closed by @charliermarsh on 2023-05-13 14:28_

---

_Comment by @alonme on 2023-05-13 15:27_

@charliermarsh thanks!
remember to cross that off the list 😄 

---

_@youknowone reviewed on 2023-05-18 21:20_

---

_Review comment by @youknowone on `crates/ruff/src/rules/pylint/rules/duplicate_bases.rs`:40 on 2023-05-18 21:20_

```suggestion
        if let Some(name) = base.node.expr_name() {
            if !seen.insert(name.id) {
                checker.diagnostics.push(Diagnostic::new(
                    DuplicateBases {
                        base: id.to_string(),
                        class: name.to_string(),
                    },
                    base.range(),
                ));
            }
        }
```

Since this is going to be used as an example, I leave a possible other form of expr matching.

---
