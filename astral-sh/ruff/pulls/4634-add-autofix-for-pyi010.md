```yaml
number: 4634
title: Add autofix for PYI010
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
  - fixes
assignees: []
merged: true
base: main
head: feature/flake8_pyi_autofixes
created_at: 2023-05-24T14:57:37Z
updated_at: 2023-05-25T08:09:02Z
url: https://github.com/astral-sh/ruff/pull/4634
synced_at: 2026-01-12T15:55:16Z
```

# Add autofix for PYI010

---

_@qdegraaf_

## Summary

Adds autofix for:

- PYI010

## Test Plan

- `cargo test` and reviewing the snapshots

I wanted to check the fix for multi-line funcs to, but this didn't work as PYI010 does not trigger on multi-line funcs quite explicitly due to:
```rust
/// PYI010
pub(crate) fn non_empty_stub_body(checker: &mut Checker, body: &[Stmt]) {
    if body.len() != 1 {
        return;
    }
```

I assume this is because [Y048](https://github.com/PyCQA/flake8-pyi/blob/ceab86d16b822d12abae1d8087850d0bc66d2f52/pyi.py#LL2107C7-L2107C7) checks and makes sure that all functions are one line. This rule does not exist in Ruff (yet). Unsure whether there was a specific reason for that. I could add the rule (it's an easy check) but then the question would be what the autofix should do if one rule is turned on and the other is turned off. It seems to me both the fix and check could be combined into one (i.e. if all stub funcs should only be `...` then all stub funcs which are anything but `...` and any amount of lines, should trigger a Violation and could be replaced by `...`). Let me know what is preferred here and I'll implement that 


---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/prefix_type_params.rs`:104 on 2023-05-24 15:44_

If the type parameter is referenced elsewhere in the file, this change would break those references, right?

---

_@charliermarsh reviewed on 2023-05-24 15:44_

---

_Comment by @charliermarsh on 2023-05-24 15:46_

Generally, our practice would be to implement Y048 as-is, and then just keep the logic independent (so we might run into cases where one rule is enabled and another isn't, and we miss out on an opportunity to fix -- but that's fine.)

---

_Comment by @github-actions[bot] on 2023-05-24 15:56_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.15ms     2.9 MB/sec    1.00     14.1±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    421.7±0.45µs     7.0 MB/sec    1.00    423.3±0.57µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.00      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.01      6.9±0.04ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1500.5±4.32µs    11.1 MB/sec    1.01   1508.5±6.23µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.07    181.5±4.16µs    16.3 MB/sec    1.00    169.3±0.17µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.02ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.1±0.01ms     7.9 MB/sec    1.16      6.0±0.00ms     6.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1014.1±1.38µs    16.4 MB/sec    1.13   1150.6±0.66µs    14.5 MB/sec
parser/numpy/globals.py                    1.00    103.7±0.41µs    28.4 MB/sec    1.10    114.3±0.17µs    25.8 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.5 MB/sec    1.14      2.5±0.00ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     18.4±0.25ms     2.2 MB/sec    1.00     18.1±0.33ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.6±0.13ms     3.6 MB/sec    1.00      4.5±0.10ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.02   530.0±12.00µs     5.6 MB/sec    1.00   518.9±11.31µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.7±0.12ms     3.3 MB/sec    1.00      7.6±0.16ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.11ms     4.6 MB/sec    1.00      8.7±0.10ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1833.9±24.36µs     9.1 MB/sec    1.00  1833.6±22.24µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    209.3±5.22µs    14.1 MB/sec    1.02    212.8±7.32µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.05ms     6.6 MB/sec    1.02      4.0±0.18ms     6.5 MB/sec
parser/large/dataset.py                    1.00      6.6±0.07ms     6.2 MB/sec    1.00      6.6±0.07ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1252.6±17.42µs    13.3 MB/sec    1.00  1251.4±18.02µs    13.3 MB/sec
parser/numpy/globals.py                    1.00    127.2±2.09µs    23.2 MB/sec    1.00    126.8±2.03µs    23.3 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.04ms     9.0 MB/sec    1.00      2.8±0.03ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Converted to draft by @qdegraaf on 2023-05-24 16:17_

---

_@qdegraaf reviewed on 2023-05-24 16:20_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_pyi/rules/prefix_type_params.rs`:104 on 2023-05-24 16:20_

Hmmm, you are correct. Fixing that might be non-trivial. Will think on it. Setting this PR to draft for now and will probably separate the two autofixes, and implement Y048 in between.

---

_Comment by @qdegraaf on 2023-05-24 16:27_

> Generally, our practice would be to implement Y048 as-is, and then just keep the logic independent (so we might run into cases where one rule is enabled and another isn't, and we miss out on an opportunity to fix -- but that's fine.)

Sounds good, I'll implement YO48. But do we want to make this rule flag on a multi-line function too? Even if you do not autofix, right now a stub function: 

```python
def foo():
    print("bla")
    print("bla2")
```

Will not flag a rule called `NonEmptyStubBody`
but this will:
```python
def foo():
    print("bla")
```

The [original plugin](https://github.com/PyCQA/flake8-pyi/blob/ceab86d16b822d12abae1d8087850d0bc66d2f52/pyi.py#L1882-L1895) goes:
```python
        if len(body) > 1:
            self.error(body[1], Y048)
        elif body:
            statement = body[0]
            # normally, should just be "..."
            if isinstance(statement, ast.Pass):
                self.error(statement, Y009)
            # Ellipsis is fine. Str (docstrings) is not but we produce
            # tailored error message for it elsewhere.
            elif not (
                isinstance(statement, ast.Expr)
                and isinstance(statement.value, (ast.Ellipsis, ast.Str))
            ):
                self.error(statement, Y010)
```
So it assumes before checking for Y010 that the Y048 does not happen. Ruff allows for a more modular approach so it might make sense to not make any assumptions about how many lines the func is when checking for PYI010.

---

_Renamed from "Add autofixes for PYI001 and PYI010" to "Add autofix PYI010" by @qdegraaf on 2023-05-24 20:43_

---

_Renamed from "Add autofix PYI010" to "Add autofix for PYI010" by @qdegraaf on 2023-05-24 20:43_

---

_@charliermarsh reviewed on 2023-05-24 20:46_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/prefix_type_params.rs`:104 on 2023-05-24 20:46_

We actually do now have the ability to track all read-references of a given symbol, so we could support this, but it would require some further changes (e.g., we'd need to run this rule _after_ visiting the entire file). I might use it as an excuse to figure out how to make it work.

---

_Comment by @qdegraaf on 2023-05-24 20:47_

@charliermarsh I think I'll just leave the autofix for PYI010 like so. If and when https://github.com/charliermarsh/ruff/pull/4645 makes it in, the warning for the multi-line scenario is covered there (and an autofix can be added for that scenario as well). In case a user willingly disables that rule but enables this one, I guess it's ok for Ruff to assume the user knows they could miss a Violation. For future rules and autofixes in this plugin I'll keep the logic independent for 

---

_Comment by @charliermarsh on 2023-05-24 20:48_

@qdegraaf - Yeah sounds good -- this looks correct to me given the above.

---

_@qdegraaf reviewed on 2023-05-24 20:49_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_pyi/rules/prefix_type_params.rs`:104 on 2023-05-24 20:49_

Separated, making this PR only for PYI010.  Not enough brainspace this evening to come up with a solution, but no need to block the other autofix. If the overhead of more but smaller PRs is fine for you and fellow maintainers, I'll cut my contributions into single chunks to avoid too much PR scope change in the future.

---

_Marked ready for review by @qdegraaf on 2023-05-24 21:19_

---

_Label `rule` added by @charliermarsh on 2023-05-24 22:10_

---

_Label `autofix` added by @charliermarsh on 2023-05-24 22:10_

---

_Merged by @charliermarsh on 2023-05-24 22:17_

---

_Closed by @charliermarsh on 2023-05-24 22:17_

---

_Branch deleted on 2023-05-25 08:09_

---
