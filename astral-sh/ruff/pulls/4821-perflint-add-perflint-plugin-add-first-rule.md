```yaml
number: 4821
title: "[`perflint`] Add `perflint` plugin, add first rule `PERF102`"
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
assignees: []
merged: true
base: main
head: feature/perflint
created_at: 2023-06-02T23:56:30Z
updated_at: 2023-06-15T11:16:06Z
url: https://github.com/astral-sh/ruff/pull/4821
synced_at: 2026-01-12T15:55:16Z
```

# [`perflint`] Add `perflint` plugin, add first rule `PERF102`

---

_@qdegraaf_

## Summary

Adds boilerplate for implementing the [perflint](https://github.com/tonybaloney/perflint/) plugin, plus a first rule. 

- [x] ~~Add check that `items()` is called on a `dict`~~ 
  - Upstream implementation in `perflint` does not do this, neither does a similar check in Ruff (`SIM118`). Code checks for some heuristics (no args, return value is an iterable of tuples etc., unpacked into two vars one of which is dummied, etc.) which should minimise the risk. See also discussion in Discord. @charliermarsh it is an open question whether or not it is a good idea to autofix in this situation, I'll leave that call to you. Can remove if too risky.

- [x] Add autofix

## Test Plan

Fixture added for PER8102

## Issue link

Refers: https://github.com/charliermarsh/ruff/issues/4789


---

_Comment by @github-actions[bot] on 2023-06-03 00:28_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+8, -0, 0 error(s))

<details><summary>airflow (+6, -0)</summary>
<p>

```diff
+ airflow/utils/log/trigger_handler.py:115:21: PERF102 [*] When using only the values of a dict use the `values()` method
+ tests/kubernetes/test_pod_generator.py:564:21: PERF102 [*] When using only the values of a dict use the `values()` method
+ tests/providers/amazon/aws/waiters/test_custom_waiters.py:101:24: PERF102 [*] When using only the keys of a dict use the `keys()` method
+ tests/providers/amazon/aws/waiters/test_custom_waiters.py:72:24: PERF102 [*] When using only the keys of a dict use the `keys()` method
+ tests/serialization/test_dag_serialization.py:380:21: PERF102 [*] When using only the values of a dict use the `values()` method
+ tests/www/views/test_views_acl.py:124:24: PERF102 [*] When using only the keys of a dict use the `keys()` method
```

</p>
</details>
<details><summary>bokeh (+2, -0)</summary>
<p>

```diff
+ src/bokeh/embed/util.py:333:30: PERF102 [*] When using only the values of a dict use the `values()` method
+ src/bokeh/models/sources.py:541:25: PERF102 [*] When using only the values of a dict use the `values()` method
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.4±0.02ms     6.4 MB/sec    1.00      6.3±0.01ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.01   1327.8±3.94µs    12.5 MB/sec    1.00   1316.9±2.62µs    12.6 MB/sec
formatter/numpy/globals.py                 1.01    128.9±0.32µs    22.9 MB/sec    1.00    127.5±0.29µs    23.1 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.00ms     9.9 MB/sec    1.00      2.6±0.02ms     9.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.08ms     3.0 MB/sec    1.00     13.8±0.10ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    414.0±1.18µs     7.1 MB/sec    1.00    414.7±1.41µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.03ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.1 MB/sec    1.00      6.7±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1443.1±3.90µs    11.5 MB/sec    1.00   1446.6±3.63µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.2±0.25µs    18.5 MB/sec    1.01    160.1±0.70µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.02ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.5±0.02ms     6.3 MB/sec    1.07      7.0±2.25ms     5.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1286.5±7.73µs    12.9 MB/sec    1.01  1297.9±35.90µs    12.8 MB/sec
formatter/numpy/globals.py                 1.01    124.4±9.32µs    23.7 MB/sec    1.00    123.0±1.46µs    24.0 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.01ms     9.7 MB/sec    1.00      2.6±0.01ms     9.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.05ms     3.1 MB/sec    1.06     14.0±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.04      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    342.3±3.35µs     8.6 MB/sec    1.04    354.4±2.11µs     8.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.03ms     4.5 MB/sec    1.05      5.9±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     5.9 MB/sec    1.13      7.8±0.04ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1382.7±5.95µs    12.0 MB/sec    1.11  1537.6±14.58µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    149.0±0.92µs    19.8 MB/sec    1.08    160.8±2.16µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.02ms     8.4 MB/sec    1.11      3.4±0.02ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @qdegraaf on 2023-06-03 10:16_

---

_Comment by @auscompgeek on 2023-06-03 12:58_

FWIW perflint's code prefix seems to be `W8xxx`

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/perflint/rules/incorrect_dict_iterator.rs`:99 on 2023-06-03 22:27_

You should be able to remove the length check here because your match pattern only matches for exactly two elements 

---

_@MichaReiser reviewed on 2023-06-03 22:28_

---

_Comment by @qdegraaf on 2023-06-04 11:13_

> FWIW perflint's code prefix seems to be `W8xxx`

Ah yeah, was worried that might clash with `Pycodestyle` plugin but that stops at `W6XX` both in the Ruff port and in the [original upstream implementation](https://pycodestyle.pycqa.org/en/latest/intro.html#error-codes) by the looks of it. I'll make the prefix `W8` 

---

_Renamed from "`[perflint]` Add `perflint` plugin, add first rule PER8102" to "`[perflint]` Add `perflint` plugin, add first rule W8102" by @qdegraaf on 2023-06-04 11:21_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-06-05 06:18_

---

_Review comment by @dhruvmanila on `crates/ruff/resources/test/fixtures/perflint/W8102.py`:1 on 2023-06-08 05:42_

Thanks for working on this. Can we add some more test cases?

```python
# Parenthesis
for (key, value) in some_dict.items():
	...

for (key, _) in some_dict.items():
	...

for (_, value) in some_dict.items():
	...

# Single variable with `items`
for pair in some_dict.items():
	...
```

There are bunch of real world cases which can be used for testing purposes: https://sourcegraph.com/search?q=context:global+lang:Python+for+%5C(_,+_%5C)+in+%5Cw+%5C.items&patternType=regexp&sm=1&groupBy=repo
* Key/value is a tuple where some elements are ignored
* Key/value is a tuple but using `(_, _)` to ignore instead of `_`

---

_@dhruvmanila reviewed on 2023-06-08 05:43_

---

_Review comment by @qdegraaf on `crates/ruff/resources/test/fixtures/perflint/W8102.py`:1 on 2023-06-08 09:57_

Good catch thanks! I have added the extra test cases. Current implementation does not catch these yet so will find some time to come up with something clean to check for all three scenarios:
```
ExprName, ExprName
ExprName, ExprTuple
ExprTuple, ExprTuple
```

---

_@qdegraaf reviewed on 2023-06-08 09:57_

---

_Converted to draft by @qdegraaf on 2023-06-08 09:57_

---

_Review comment by @qdegraaf on `crates/ruff/resources/test/fixtures/perflint/W8102.py`:1 on 2023-06-08 10:23_

Seeing as the tuples could be N-layers deep, recursion would seem a good fit and would probably be the first thing I try. I'm not familiar with Rust's attitude towards recursive functions though, or whether it has a better solution 

---

_@qdegraaf reviewed on 2023-06-08 10:23_

---

_@qdegraaf reviewed on 2023-06-08 19:56_

---

_Review comment by @qdegraaf on `crates/ruff/resources/test/fixtures/perflint/W8102.py`:1 on 2023-06-08 19:56_

New edits should catch these scenarios. I'll see if I can make it a bit less clunky and verbose.

---

_Marked ready for review by @qdegraaf on 2023-06-08 19:56_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/perflint/rules/incorrect_dict_iterator.rs`:76 on 2023-06-09 01:57_

nit: Can we use the `let ... else { return }` pattern instead of `if let ... { ... }` to avoid nesting. It'll also make the code linear and easier to read :)

I think you're already using this pattern in a few other places.

```rust
    let Expr::Call(ast::ExprCall { func, args, .. }) = iter else {
    	return;
    }
```

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/perflint/rules/incorrect_dict_iterator.rs`:88 on 2023-06-09 01:59_

nit: explicit names are good for posterity :)

```suggestion
            range: tuple_range,
```

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/perflint/rules/incorrect_dict_iterator.rs`:129 on 2023-06-09 02:30_

Shouldn't this be outside the `if let Expr::Name(...` block? The same question for other branches as well.

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/perflint/rules/incorrect_dict_iterator.rs`:59 on 2023-06-09 02:47_

nit: Would `is_ignored_tuple_or_name` be better? I prefer to avoid negation in the name for better readability. Then the return comparison will be simpler: `id == "_"`. This would also make the recursive case easier to reason about.

```suggestion
fn is_ignored_tuple_or_name(expr: &Expr) -> bool {
    if let Expr::Name(ast::ExprName { id, .. }) = expr {
        return id == "_";
    }

    if let Expr::Tuple(ast::ExprTuple { elts, .. }) = expr {
        return elts.iter().all(is_ignored_tuple_or_name);
    }
    false
}

```

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/perflint/rules/incorrect_dict_iterator.rs`:141 on 2023-06-09 02:49_

Using `is_ignored_tuple_or_name` would make this simpler as well. So, instead of negating the result here, we'll do it in the function itself.

```suggestion
                    if !sub_elts
                        .iter()
                        .all(is_ignored_tuple_or_name)
```

If you think otherwise, using `is_not_ignored_tuple_or_name` could be simplified as:
```suggestion
                    if sub_elts
                        .iter()
                        .any(is_not_ignored_tuple_or_name)
```

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/perflint/rules/incorrect_dict_iterator.rs`:223 on 2023-06-09 02:50_

Same as above
```suggestion
                    if !sub_elts
                        .iter()
                        .all(is_ignored_tuple_or_name)
```

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/perflint/snapshots/ruff__rules__perflint__tests__W8102_W8102.py.snap`:9 on 2023-06-09 02:55_

I would say the diagnostics should be at `.items()` instead, the `_` is the reason due to which the violation happens. And we're also suggesting to replace `.items()`. What do you think?

---

_@dhruvmanila reviewed on 2023-06-09 02:56_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/perflint/rules/incorrect_dict_iterator.rs`:129 on 2023-06-10 09:30_

Depends on what we want the behaviour to be. Current implementation does not continue if it finds a violation for the first of the two args. So a case of 

```python
for _, _ in d.items():
    pass
```

Will trigger a violation only for the first arg. Do we think it should be the responsibility of this rule to check for a completely ignored `.items()` call? And if so what do we want to do with autofixes?

---

_@qdegraaf reviewed on 2023-06-10 09:30_

---

_@qdegraaf reviewed on 2023-06-10 09:57_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/perflint/rules/incorrect_dict_iterator.rs`:76 on 2023-06-10 09:57_

Good suggestion! Done it for a bunch of `if let`'s now. Could go one or two steps further (and now maybe refactoring to some `fn` that can be called on both elements instead of having 4 branches of code for both scenarios for both elements). But already liking its readability much more so your call AFAIAC

---

_Review requested from @dhruvmanila by @dhruvmanila on 2023-06-10 16:35_

---

_Review comment by @konstin on `crates/ruff/src/rules/perflint/rules/incorrect_dict_iterator.rs`:35 on 2023-06-12 10:30_

Can this be a `&'static str`?

---

_Review comment by @konstin on `crates/ruff/src/rules/perflint/rules/incorrect_dict_iterator.rs`:69 on 2023-06-12 10:33_

nit:

```suggestion
        return;
    };
```

rustfmt unfortunately doesn't support let-else yet

---

_@konstin reviewed on 2023-06-12 12:07_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/perflint/rules/incorrect_dict_iterator.rs`:35 on 2023-06-12 12:37_

Unsure. It's a `String` for all other Violations I have found so I just stuck to that. Unsure if there is a specific reason for those violations to use `String` instead of `&'static str` that is not valid in this case.

---

_@qdegraaf reviewed on 2023-06-12 12:37_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/perflint/rules/incorrect_dict_iterator.rs`:117 on 2023-06-12 17:00_

I think we could convert this into a function which can accept a `subset` parameter which could be an `enum` instead of a string and an `expr` parameter which would be the other expression node.

We could also simplify the extraction of `fix_val` to avoid creating an invalid state where the other element isn't a `Name` or `Tuple` node.

```suggestion
                    let fix_val = match &elts[1] {
                        Expr::Name(ast::ExprName { id, .. }) => id.as_str(),
                        Expr::Tuple(ast::ExprTuple {
                            range: val_range, ..
                        }) => checker.locator.slice(*val_range),
                        _ => panic!("Expected Expr::Name | Expr::Tuple"),
                    };
                    diagnostic.set_fix(Fix::automatic_edits(
                        Edit::range_replacement(".values".to_string(), method_range),
                        [Edit::range_replacement(fix_val.to_string(), *tuple_range)],
                    ));
```

---

_Review comment by @dhruvmanila on `crates/ruff/src/registry.rs`:195 on 2023-06-12 17:06_

I'm not sure about the prefix here as we would want a 3 letter abbreviation for the plugin name (`PER`, `PRF`?). It's ok to deviate from the actual rule code for consistency :)

\cc @charliermarsh 

---

_@dhruvmanila approved on 2023-06-12 17:06_

---

_@dhruvmanila reviewed on 2023-06-12 17:13_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/perflint/rules/incorrect_dict_iterator.rs`:129 on 2023-06-12 17:13_

This is an interesting case. `perflint` seems to be triggering for both key and value place which doesn't seem correct to me. I guess the reason someone would want to use a loop like this is to do something `n` number of times where `n` is the dictionary length. I would suggest to ignore such cases plus I don't think it's the responsibility of this rule to check it.

---

_Comment by @charliermarsh on 2023-06-12 18:59_

Will give this a pass and merge today.

---

_Review comment by @charliermarsh on `crates/ruff/src/registry.rs`:195 on 2023-06-12 18:59_

Maybe `PERF`? We've started to use some four-letter prefixes.

---

_@charliermarsh reviewed on 2023-06-12 18:59_

---

_Comment by @qdegraaf on 2023-06-12 20:26_

> Will give this a pass and merge today.

Let me know if any input is requested. Happy to step aside though, earliest I can get round to @dhruvmanila's comments and extracting some stuff to a function is tomorrow evening (GMT+1) 

---

_@charliermarsh reviewed on 2023-06-13 01:18_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/perflint/rules/incorrect_dict_iterator.rs`:35 on 2023-06-13 01:18_

I recommend using an `enum` in cases like this (see below).

---

_Renamed from "`[perflint]` Add `perflint` plugin, add first rule W8102" to "[`perflint`] Add `perflint` plugin, add first rule `PERF102`" by @charliermarsh on 2023-06-13 01:46_

---

_Label `rule` added by @charliermarsh on 2023-06-13 01:46_

---

_Merged by @charliermarsh on 2023-06-13 01:54_

---

_Closed by @charliermarsh on 2023-06-13 01:54_

---

_Branch deleted on 2023-06-15 11:16_

---
