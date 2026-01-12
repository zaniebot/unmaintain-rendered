```yaml
number: 4200
title: "[`pylint`] Implement `nested-min-max` (`W3301`)"
type: pull_request
state: merged
author: mccullocht
labels: []
assignees: []
merged: true
base: main
head: nested_min_max
created_at: 2023-05-03T05:10:02Z
updated_at: 2023-05-15T14:35:20Z
url: https://github.com/astral-sh/ruff/pull/4200
synced_at: 2026-01-12T15:55:14Z
```

# [`pylint`] Implement `nested-min-max` (`W3301`)

---

_@mccullocht_

Detect nested calls to `min()` or `max()` and generate fixes for these, e.g. `min(1, min(2, 3)) => min(1, 2, 3)`

Behavior differs from pylint when keyword args are present -- we skip analysis in these cases as it may not
actually be correct to to merge the nested statements. The following statements would not receive a diagnostic
in this implementation, but do in pylint:
```
min(1, min(2, 3), key=test) => min(1, 2, 3, key=test)
min(1, min(2, 3, key=test)) => min(1, 2, 3)
```

This is part of #970 

---

_Comment by @github-actions[bot] on 2023-05-03 05:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     19.3±0.53ms     2.1 MB/sec    1.00     19.0±0.44ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.14ms     3.7 MB/sec    1.00      4.5±0.16ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   553.0±18.69µs     5.3 MB/sec    1.00   552.1±16.14µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.9±0.34ms     3.2 MB/sec    1.00      7.8±0.20ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.03      9.5±0.35ms     4.3 MB/sec    1.00      9.3±0.20ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1977.6±60.81µs     8.4 MB/sec    1.00  1931.5±37.35µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.03    229.2±9.10µs    12.9 MB/sec    1.00   223.0±10.00µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.2±0.10ms     6.1 MB/sec    1.00      4.1±0.17ms     6.2 MB/sec
parser/large/dataset.py                    1.00      7.3±0.12ms     5.6 MB/sec    1.01      7.4±0.15ms     5.5 MB/sec
parser/numpy/ctypeslib.py                  1.00  1419.0±34.02µs    11.7 MB/sec    1.02  1443.3±42.07µs    11.5 MB/sec
parser/numpy/globals.py                    1.00    139.3±4.63µs    21.2 MB/sec    1.02    141.8±6.26µs    20.8 MB/sec
parser/pydantic/types.py                   1.00      3.1±0.07ms     8.2 MB/sec    1.01      3.2±0.08ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     21.1±0.74ms  1970.3 KB/sec    1.00     21.2±0.74ms  1967.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.3±0.27ms     3.2 MB/sec    1.00      5.2±0.18ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   627.7±26.18µs     4.7 MB/sec    1.00   620.9±29.63µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.7±0.31ms     2.9 MB/sec    1.00      8.7±0.31ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.2±0.33ms     4.0 MB/sec    1.01     10.3±0.28ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.09ms     7.7 MB/sec    1.01      2.2±0.12ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   259.8±18.49µs    11.4 MB/sec    1.01   262.6±13.47µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.30ms     5.5 MB/sec    1.00      4.6±0.18ms     5.5 MB/sec
parser/large/dataset.py                    1.00      8.4±0.22ms     4.8 MB/sec    1.00      8.4±0.17ms     4.8 MB/sec
parser/numpy/ctypeslib.py                  1.00  1618.7±64.56µs    10.3 MB/sec    1.00  1613.7±66.96µs    10.3 MB/sec
parser/numpy/globals.py                    1.01    167.7±9.26µs    17.6 MB/sec    1.00    165.9±8.30µs    17.8 MB/sec
parser/pydantic/types.py                   1.02      3.7±0.15ms     7.0 MB/sec    1.00      3.6±0.11ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/nested_min_max.rs`:1 on 2023-05-03 09:21_

Delete?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/nested_min_max.rs`:83 on 2023-05-03 09:29_

We should assert that `min` and `max` are indeed the built-in `min` and `max` function. You can test this by calling `checker.ctx.is_builtin("min");`

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/nested_min_max.rs`:73 on 2023-05-03 09:35_

What happens if there are any comments attached to the arguments

```python
min(
	# Important 
	min(2, 3),
	# more comments
	5,
)
```

---

_@MichaReiser reviewed on 2023-05-03 09:36_

Thank you for working on this. This is looking good to me. I think we should improve the error messages to make them more actionable and we should ensure to not flag any user defined `min` or `max` function calls.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/nested_min_max.rs`:73 on 2023-05-04 02:45_

They'll get removed. Any fix that goes through `unparse_expr` will remove comments. We often skip autofixes when expressions do contain comments for this reason -- you can grep for `has_comments_in` to see examples of that pattern.

---

_@charliermarsh reviewed on 2023-05-04 02:46_

---

_Review comment by @mccullocht on `crates/ruff/src/rules/pylint/rules/nested_min_max.rs`:73 on 2023-05-05 04:52_

No longer provides a fix if there are comments within expr and added a test.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/nested_min_max.rs`:45 on 2023-05-05 06:56_

This rule should no longer implement `AlwaysAutofixableViolation` because it doesn't provide a fix if there are comments.

---

_@MichaReiser reviewed on 2023-05-05 06:56_

---

_Review comment by @mccullocht on `crates/ruff/src/rules/pylint/rules/nested_min_max.rs`:83 on 2023-05-06 04:48_

done

---

_Review comment by @mccullocht on `crates/ruff/src/rules/pylint/rules/nested_min_max.rs`:45 on 2023-05-06 20:13_

Fixed

---

_@mccullocht reviewed on 2023-05-06 20:14_

---

_Review requested from @MichaReiser by @mccullocht on 2023-05-06 20:17_

---

_Review requested from @charliermarsh by @mccullocht on 2023-05-06 20:17_

---

_Renamed from "Implement pylint `nested-min-max / W3301`" to "[`pylint`] Implement `nested-min-max` (`W3301`)" by @charliermarsh on 2023-05-07 03:07_

---

_Merged by @charliermarsh on 2023-05-07 03:14_

---

_Closed by @charliermarsh on 2023-05-07 03:14_

---

_Comment by @nijel on 2023-05-15 14:35_

Automatic fix breaks when iterables are involved, see https://github.com/charliermarsh/ruff/issues/4440

---
