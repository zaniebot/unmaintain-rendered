```yaml
number: 4075
title: "[`pylint`] Implement PLE0302 `unexpected-special-method-signature`"
type: pull_request
state: merged
author: mccullocht
labels:
  - rule
assignees: []
merged: true
base: main
head: unexpected-special-method-signature
created_at: 2023-04-24T04:51:57Z
updated_at: 2023-04-25T05:23:40Z
url: https://github.com/astral-sh/ruff/pull/4075
synced_at: 2026-01-12T15:55:14Z
```

# [`pylint`] Implement PLE0302 `unexpected-special-method-signature`

---

_@mccullocht_

Implement pylint [`unexpected-special-method-signature`](https://pylint.readthedocs.io/en/latest/user_guide/messages/error/unexpected-special-method-signature.html)

This is part of issue https://github.com/charliermarsh/ruff/issues/970

---

_Comment by @charliermarsh on 2023-04-24 05:04_

Nice, thanks for the PR! Look forward to reviewing :)

---

_Comment by @github-actions[bot] on 2023-04-24 05:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     15.4±0.04ms     2.6 MB/sec    1.00     15.1±0.02ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.9±0.01ms     4.3 MB/sec    1.00      3.8±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.03    431.4±5.62µs     6.8 MB/sec    1.00    418.3±2.56µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.6±0.02ms     3.9 MB/sec    1.00      6.5±0.01ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.04      8.1±0.02ms     5.0 MB/sec    1.00      7.8±0.01ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1775.7±5.29µs     9.4 MB/sec    1.00   1742.1±3.33µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    185.4±0.51µs    15.9 MB/sec    1.00    183.5±0.85µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.7±0.01ms     6.8 MB/sec    1.00      3.6±0.01ms     7.0 MB/sec
parser/large/dataset.py                    1.04      6.5±0.00ms     6.3 MB/sec    1.00      6.2±0.01ms     6.5 MB/sec
parser/numpy/ctypeslib.py                  1.02   1273.9±1.44µs    13.1 MB/sec    1.00   1248.1±2.03µs    13.3 MB/sec
parser/numpy/globals.py                    1.01    127.4±0.23µs    23.2 MB/sec    1.00    125.9±0.55µs    23.4 MB/sec
parser/pydantic/types.py                   1.02      2.8±0.00ms     9.2 MB/sec    1.00      2.7±0.00ms     9.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     17.3±0.20ms     2.3 MB/sec    1.00     17.0±0.16ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.4±0.04ms     3.8 MB/sec    1.00      4.4±0.04ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.02   542.7±13.75µs     5.4 MB/sec    1.00    534.1±5.64µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.3±0.09ms     3.5 MB/sec    1.00      7.3±0.13ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.03      8.9±0.07ms     4.6 MB/sec    1.00      8.7±0.10ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1938.6±17.24µs     8.6 MB/sec    1.00  1906.0±24.51µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.01    217.8±3.32µs    13.5 MB/sec    1.00    216.3±7.11µs    13.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.0±0.04ms     6.3 MB/sec    1.00      4.0±0.05ms     6.4 MB/sec
parser/large/dataset.py                    1.00      6.9±0.05ms     5.9 MB/sec    1.00      6.8±0.04ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1313.2±13.86µs    12.7 MB/sec    1.00  1308.4±13.68µs    12.7 MB/sec
parser/numpy/globals.py                    1.00    131.4±2.79µs    22.5 MB/sec    1.02    133.9±2.63µs    22.0 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.03ms     8.6 MB/sec    1.00      2.9±0.03ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `rule` added by @charliermarsh on 2023-04-24 05:24_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/unexpected_special_method_signature.rs`:121 on 2023-04-24 05:28_

Does this need to take into account positional-only and/or keyword-only arguments? (Elsewhere, we have logic like `let defaults_start = args.posonlyargs.len() + args.args.len() - args.defaults.len();` and `let defaults_start = args.kwonlyargs.len() - args.kw_defaults.len();` to infer some similar information from function signatures.)

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/unexpected_special_method_signature.rs`:50 on 2023-04-24 05:31_

Rather than modeling each case as `RangeInclusive`, and using `self.expected_params.end() - self.expected_params.start() > 1`-like checks to determine whether a branch should be treated as a range or not, what if we introduced an enum here to represent the two cases? E.g.:

```rust
enum ExpectedParameters {
  Fixed(usize),
  // Or RangeInclusive<usize>
  Range(usize, usize),
}
```

(Names etc. are of course flexible, just illustrating the concept.)

I think that would make some of the case-handling below a bit clearer by way of being more explicit.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/unexpected_special_method_signature.rs`:40 on 2023-04-24 05:32_

Nit: consider using either `parameter` or `parameters` based on `self.expected_params.start()`, rather than `param(s)`? It's a little more code (one more case) but we tend to do it elsewhere.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/unexpected_special_method_signature.rs`:81 on 2023-04-24 05:33_

(This could even be an associated function on `ExpectedParameters`, to increment the count.)

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/unexpected_special_method_signature.rs`:108 on 2023-04-24 05:33_

(Nit: you could choose to use `let-else` here to avoid the nesting, but totally up to you.)

---

_@charliermarsh reviewed on 2023-04-24 05:33_

Nice PR! Thanks for getting involved. One question below and a couple small comments.

---

_Review comment by @mccullocht on `crates/ruff/src/rules/pylint/rules/unexpected_special_method_signature.rs`:108 on 2023-04-24 18:40_

Done

---

_Review comment by @mccullocht on `crates/ruff/src/rules/pylint/rules/unexpected_special_method_signature.rs`:121 on 2023-04-24 18:43_

I patterned off of the [pylint check](https://github.com/pylint-dev/pylint/blob/bc9c5c6610cce2a068eabce598200399b9d002f2/pylint/checkers/classes/special_methods_checker.py#L197) which did not take posonly/kwonly args into account, but I can investigate further if you'd like. I don't think we need to consider kw-only arguments as it doesn't seem that any special methods have them according to the [documentation](https://docs.python.org/2/reference/datamodel.html#special-method-names).

---

_Review comment by @mccullocht on `crates/ruff/src/rules/pylint/rules/unexpected_special_method_signature.rs`:50 on 2023-04-24 19:13_

Done.

I used the new enum in the violation so it has to be public :/. If you have a preference `expected_params` can be rendered into a string for formatting before creating the violation, which would allow `ExpectedParams` to be private again.

---

_Review comment by @mccullocht on `crates/ruff/src/rules/pylint/rules/unexpected_special_method_signature.rs`:40 on 2023-04-24 19:13_

Done.

I was using the copy from pylint as a starting point but this is clearer.

---

_Review comment by @mccullocht on `crates/ruff/src/rules/pylint/rules/unexpected_special_method_signature.rs`:81 on 2023-04-24 19:14_

`ExpectedParams::from_method()` takes an `is_staticmethod` bool and applies this internally on construction, but I got rid of the `map()` call.

---

_@mccullocht reviewed on 2023-04-24 19:22_

Thank you for the prompt review!

---

_Review requested from @charliermarsh by @mccullocht on 2023-04-24 19:23_

---

_@charliermarsh reviewed on 2023-04-25 04:47_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/unexpected_special_method_signature.rs`:121 on 2023-04-25 04:47_

Perhaps we want to extend the check with a custom message when posonly/kwonly args are included, since that itself deviates from the expected signature, albeit in a different way?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/unexpected_special_method_signature.rs`:50 on 2023-04-25 04:47_

All good, we follow this pattern after and the enums end up public. No worries.

---

_@charliermarsh reviewed on 2023-04-25 04:47_

---

_Merged by @charliermarsh on 2023-04-25 04:51_

---

_Closed by @charliermarsh on 2023-04-25 04:51_

---

_Comment by @charliermarsh on 2023-04-25 04:52_

Thanks so much for the contribution!

---
