```yaml
number: 5313
title: "[`perflint`] Add `PERF403`"
type: pull_request
state: closed
author: qdegraaf
labels: []
assignees: []
draft: true
base: main
head: feature/PERF403
created_at: 2023-06-22T19:32:18Z
updated_at: 2023-07-27T15:12:01Z
url: https://github.com/astral-sh/ruff/pull/5313
synced_at: 2026-01-12T03:30:21Z
```

# [`perflint`] Add `PERF403`

---

_Pull request opened by @qdegraaf on 2023-06-22 19:32_

## Summary

Adds `PERF403` mirroring `W8403` from https://github.com/tonybaloney/perflint 

Implementation is similar to [original/upstream implementation](https://github.com/tonybaloney/perflint/blob/c07391c17671c3c9d5a7fd69120d1f570e268d58/perflint/comprehension_checker.py), with the exception of not yet checking whether the `ExprSubscript` value is a dictionary type value. I tried adding this but ended up doing a lot of 

```rust
let scope = checker.semantic().scope();
if let Some(binding_id) = scope.get(subscript_value_id) {
    let binding = &checker.semantic().bindings[binding_id];
    if binding.kind.is_assignment() || binding.kind.is_named_expr_assignment() {
        if let Some(parent_id) = binding.source {
        etc....
```
again for what felt like very little added value.

Open to any feedback/input to do this more efficiently. If there is no more efficient way to do this, also let me know and I'll see if I can make some kind of helper or util for this, as I've bumped into this a few times now with various rules. 

I've set the violation to only flag the first append call in a long `if-else` statement. Happy to change this to some other location or make it multiple violations if that makes more sense.

## Test Plan

Fixtures were added based on perflint tests

## Issue Links

Refers: https://github.com/astral-sh/ruff/issues/4789 


---

_Comment by @github-actions[bot] on 2023-06-22 19:49_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+21, -0, 0 error(s))

<details><summary>airflow (+13, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/9d7c2246815251727af6d9119d7ef7660338e8c8/airflow/dag_processing/manager.py#L997'>airflow/dag_processing/manager.py:997:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/9d7c2246815251727af6d9119d7ef7660338e8c8/airflow/executors/kubernetes_executor_utils.py#L137'>airflow/executors/kubernetes_executor_utils.py:137:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/9d7c2246815251727af6d9119d7ef7660338e8c8/airflow/providers/exasol/hooks/exasol.py#L66'>airflow/providers/exasol/hooks/exasol.py:66:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/9d7c2246815251727af6d9119d7ef7660338e8c8/airflow/providers/google/cloud/hooks/bigquery.py#L3063'>airflow/providers/google/cloud/hooks/bigquery.py:3063:13:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/9d7c2246815251727af6d9119d7ef7660338e8c8/airflow/providers/google/cloud/transfers/gcs_to_bigquery.py#L708'>airflow/providers/google/cloud/transfers/gcs_to_bigquery.py:708:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/9d7c2246815251727af6d9119d7ef7660338e8c8/airflow/providers/mysql/hooks/mysql.py#L163'>airflow/providers/mysql/hooks/mysql.py:163:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/9d7c2246815251727af6d9119d7ef7660338e8c8/airflow/providers/postgres/hooks/postgres.py#L149'>airflow/providers/postgres/hooks/postgres.py:149:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/9d7c2246815251727af6d9119d7ef7660338e8c8/airflow/providers/smtp/hooks/smtp.py#L282'>airflow/providers/smtp/hooks/smtp.py:282:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/9d7c2246815251727af6d9119d7ef7660338e8c8/airflow/utils/email.py#L215'>airflow/utils/email.py:215:13:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/9d7c2246815251727af6d9119d7ef7660338e8c8/dev/breeze/src/airflow_breeze/utils/run_utils.py#L185'>dev/breeze/src/airflow_breeze/utils/run_utils.py:185:13:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/9d7c2246815251727af6d9119d7ef7660338e8c8/setup.py#L524'>setup.py:524:9:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/9d7c2246815251727af6d9119d7ef7660338e8c8/tests/test_utils/config.py#L57'>tests/test_utils/config.py:57:13:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/9d7c2246815251727af6d9119d7ef7660338e8c8/tests/test_utils/config.py#L82'>tests/test_utils/config.py:82:13:</a> PERF403 Use a dictionary comprehension instead of a for-loop
</pre>

</p>
</details>
<details><summary>bokeh (+3, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/has_props.py#L128'>src/bokeh/core/has_props.py:128:13:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/wrappers.py#L517'>src/bokeh/core/property/wrappers.py:517:21:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/models/sources.py#L283'>src/bokeh/models/sources.py:283:13:</a> PERF403 Use a dictionary comprehension instead of a for-loop
</pre>

</p>
</details>
<details><summary>zulip (+5, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/b7fcce305e5175f3d613d1e5c8371a834d22a609/analytics/views/stats.py#L456'>analytics/views/stats.py:456:9:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/zulip/zulip/blob/b7fcce305e5175f3d613d1e5c8371a834d22a609/analytics/views/stats.py#L531'>analytics/views/stats.py:531:9:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/zulip/zulip/blob/b7fcce305e5175f3d613d1e5c8371a834d22a609/zerver/lib/response.py#L128'>zerver/lib/response.py:128:9:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/zulip/zulip/blob/b7fcce305e5175f3d613d1e5c8371a834d22a609/zerver/views/auth.py#L371'>zerver/views/auth.py:371:13:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/zulip/zulip/blob/b7fcce305e5175f3d613d1e5c8371a834d22a609/zerver/views/development/registration.py#L28'>zerver/views/development/registration.py:28:9:</a> PERF403 Use a dictionary comprehension instead of a for-loop
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PERF403 | 21 | 21 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.02ms     3.7 MB/sec    1.02     11.2±0.04ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.00ms     7.5 MB/sec    1.00      2.2±0.00ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00    247.0±0.31µs    11.9 MB/sec    1.07   264.3±12.88µs    11.2 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.01ms     5.4 MB/sec    1.02      4.8±0.01ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.03ms     2.7 MB/sec    1.02     15.5±0.02ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.00ms     4.3 MB/sec    1.01      3.9±0.01ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    510.7±0.68µs     5.8 MB/sec    1.01    516.2±3.26µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.03ms     3.7 MB/sec    1.01      7.0±0.01ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.02ms     5.2 MB/sec    1.00      7.8±0.02ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1676.4±6.23µs     9.9 MB/sec    1.00   1665.9±2.47µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    187.1±0.29µs    15.8 MB/sec    1.00    187.1±2.36µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.6±0.01ms     7.2 MB/sec    1.00      3.5±0.00ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.8±0.08ms     3.8 MB/sec    1.01     10.9±0.07ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.7 MB/sec    1.00      2.2±0.04ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    242.9±4.10µs    12.1 MB/sec    1.02   248.8±11.57µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.06ms     5.5 MB/sec    1.01      4.7±0.05ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.09ms     2.7 MB/sec    1.00     15.2±0.21ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.2 MB/sec    1.00      4.0±0.04ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    482.5±7.00µs     6.1 MB/sec    1.01    486.3±6.21µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.05ms     3.7 MB/sec    1.00      6.9±0.10ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.05ms     5.1 MB/sec    1.00      7.9±0.09ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1654.6±21.59µs    10.1 MB/sec    1.00  1654.6±14.50µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    187.9±2.47µs    15.7 MB/sec    1.01    189.6±3.71µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.03ms     7.2 MB/sec    1.00      3.5±0.03ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/perflint/rules/slow_dict_creation.rs`:69 on 2023-06-23 06:05_

Nit: the name `if_stmt` confused me because I assumed it is an `if_stmt`. But what it really is is any statement in the if body. Maybe just call it `stmt` (as you do in the `check_for_slow_dict_creation` method). 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/perflint/rules/slow_dict_creation.rs`:59 on 2023-06-23 06:06_

Nit: We could early return if `names.is_empty`

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/perflint/rules/slow_dict_creation.rs`:42 on 2023-06-23 06:10_

Nit. I believe you can merge the `if body.len() != 1` and `let stmt = &body[0]` by doing

```suggestion
	let [stmt] = body else {
		return;
	}
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/perflint/rules/slow_dict_creation.rs`:69 on 2023-06-23 06:12_

Nit. You can move the loop into the if instead of doing another early return
```suggestion
    if let Stmt::If(ast::StmtIf { body: if_body, .. }) = stmt {
	    for if_stmt in if_body {
	        check_for_slow_dict_creation(checker, names.as_slice(), if_stmt);
	    }
    };
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/perflint/rules/slow_dict_creation.rs`:75 on 2023-06-23 06:12_

The formatting of all `else` branches is off (rustfmt doesn't yet support let else chains

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/perflint/rules/slow_dict_creation.rs`:83 on 2023-06-23 06:13_

Nit: I believe this works
```suggestion
    let [Expr::Subscript(ast::ExprSubscript { slice, .. })] = targets.as_slice() else {
            return 
     };
```

---

_@MichaReiser reviewed on 2023-06-23 06:13_

---

_Review requested from @charliermarsh by @charliermarsh on 2023-06-29 01:04_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/perflint/rules/manual_dict_comprehension.rs`:73 on 2023-07-11 16:34_

Similar to `PERF401`, we should ignore this rule if the dictionary assignment variable is being used inside the conditional test: https://github.com/astral-sh/ruff/blob/30bec3fcfa01c044ea232aae9d3c69b59f422d30/crates/ruff/src/rules/perflint/rules/manual_list_comprehension.rs#L129-L152

---

_Review comment by @dhruvmanila on `crates/ruff/resources/test/fixtures/perflint/PERF403.py`:5 on 2023-07-11 16:40_

(I wonder similar to `PERF402` why there isn't the separation for dictionary, I'm assuming the corresponding rule for dictionary doesn't exists upstream as well).

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/perflint/rules/manual_dict_comprehension.rs`:66 on 2023-07-11 16:45_

We could simplify this using:
```rust
elt.as_name_expr().map_or(None, |name| Some(name.as_str()))
```

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/perflint/rules/manual_dict_comprehension.rs`:112 on 2023-07-11 16:48_

Maybe we could use `.as_named_expr` here as well with something like this:
```rust
if Some(key) = slice.as_named_expr() {
	if Some(value) = value.as_named_expr() {
		...
	}
}
```

Or, just using the `match` expression:
```rust
match (slice.as_named_expr(), value.as_named_expr()) {
	(Some(key), Some(value)) => ...,
	_ => return;
}
```

---

_@dhruvmanila approved on 2023-07-11 16:48_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/perflint/rules/manual_dict_comprehension.rs`:73 on 2023-07-11 18:16_

I think this is deviating from upstream plugin, and there's no autofix yet, but I do agree it's a good check to have. Added some extra tests (and some of previous are now invalid) and made a helper func out of the check. Got a Clippy warning about borrowing a Box there though I didn't have time to dig into, so let me know if that's not OK to allow and what I could do better there. 

---

_@qdegraaf reviewed on 2023-07-11 18:16_

---

_Review comment by @qdegraaf on `crates/ruff/resources/test/fixtures/perflint/PERF403.py`:5 on 2023-07-11 18:19_

Yeah don't know why it's split into `copy()` and `comprehension` for lists but only one for dicts. My implementation was just a straight copy from https://github.com/tonybaloney/perflint/blob/main/perflint/comprehension_checker.py 

As we are already deviating with the if conditional checks, we could add a separate rule `PERF404` if that has value.



---

_@qdegraaf reviewed on 2023-07-11 18:19_

---

_Comment by @charliermarsh on 2023-07-12 03:48_

The delay on this one is that I'm worried about false positives. Most of the examples surfaced in the ecosystem CI checks seem to be false positives. I don't think we have a great way to solve this right now. The best we can do, probably, is check if the variable to which we're assigning was assigned the empty dictionary immediately prior to the loop. That's probably a necessary precondition to merging here just based on what I see in the ecosystem CI -- wdyt, @qdegraaf?

---

_Comment by @qdegraaf on 2023-07-12 08:10_

> The delay on this one is that I'm worried about false positives. Most of the examples surfaced in the ecosystem CI checks seem to be false positives. I don't think we have a great way to solve this right now. The best we can do, probably, is check if the variable to which we're assigning was assigned the empty dictionary immediately prior to the loop. That's probably a necessary precondition to merging here just based on what I see in the ecosystem CI -- wdyt, @qdegraaf?

@charliermarsh it's been on my TODO list to look at this one again since I got wind of PERF401 and PERF402 being quite false positive heavy as well, wanted to pick it up after the Dlint/ReDoS thing but will prioritise this over that now. I am away until after the weekend but plan after that is to:

- Double check upstream implementation, and run that against the false positive snippets from the ecosystem CI to see if I missed an implementation detail
- Add false positives from the ecosystem CI to fixtures
- Expand checks (agree that best heuristic is probably to check if dict assignment happens right before the loop) until false positives don't flag

Happy to wait on merging until that's sorted and we are satisfied with the precision. If we can't get it to perform like we want I'll check in again then to see what we want to do with it. I'll set the PR to draft until I get further. 

---

_Converted to draft by @qdegraaf on 2023-07-12 08:10_

---

_Comment by @qdegraaf on 2023-07-19 19:58_

@charliermarsh 
Had a look at the upstream implementation and it appears `perflint` lets anything more complex than a simple `if/else` slide, but does not check anything with regards to the contents of the dict. It works similarly for PERF401 and PERF402, I can open a separate PR to address that if we want, however:

For false positives based on whether the dictionary is empty. `perflint` flags:
```python
def foo():
    result = {"a": 1, "b": 2}
    fruit = ["apple", "pear", "orange"]
    for idx, name in enumerate(fruit):
        if idx % 2:
            result[idx] = name
```
as
```
************* Module scratch
scratch.py:4:4: W8403: Use a dictionary comprehension instead of a for-loop (use-dict-comprehension)
scratch.py:3:12: W8301: Use tuple instead of list for a non-mutated sequence (use-tuple-over-list)

------------------------------------------------------------------
Your code has been rated at 6.67/10 (previous run: 8.46/10, -1.79)
```

We can add the heuristic. But checking other `PERF4XX` we actually have similar issues, e.g.:
```python
def f():
    items = [1, 2, 3, 4]
    result = [5, 6]
    for i in items:
        result.append(i * i) 
```
flags:
```
************* Module scratch
scratch.py:4:4: W8402: Use a list copy instead of a for-loop (use-list-copy)
scratch.py:2:12: W8301: Use tuple instead of list for a non-mutated sequence (use-tuple-over-list)

------------------------------------------------------------------
Your code has been rated at 6.00/10 (previous run: 6.67/10, -0.67)
```

So we'd have to add the heuristic to all `4XX` rules and I wouldn't exactly call it a clean fix ready to spread to multiple rules. If we don't have a way to efficiently determine whether a given `list` or `dict` is empty when checking the violation , and considering the upstream implementation is more false-positive heavy than we maybe initially thought, is it worth considering not porting the `4XX` rules at all? I should have checked the coverage of the original plug-in a bit more thoroughly before implementing. Happy to add the heuristic and fix up the three rules but just want to check if it's worth the effort to add a rough heuristic in order to patch up an already rough rule. 



---

_Comment by @charliermarsh on 2023-07-21 03:25_

I think this is okay to flag:

```python
def f():
    items = [1, 2, 3, 4]
    result = [5, 6]
    for i in items:
        result.append(i * i) 
```

Because it can be expressed as `result.extend(i * i for i in items)`.

What if the heuristic was: check if the variable was assigned to a dictionary literal prior to the loop?

---

_Comment by @qdegraaf on 2023-07-21 07:28_

Ah yeah, of course, check. Lot of context switches muddying up the mind recently. 

I'll implement that heuristic. Two questions:

* Shall I also still open a PR to exclude `if/elif/else` statements from `PERF401`/`PERF402` (and exclude them here of course) to match [upstream](https://github.com/tonybaloney/perflint/blob/c07391c17671c3c9d5a7fd69120d1f570e268d58/tests/test_comprehension_checker.py#L25-L39)?
* Do we still want the heuristic to be in the line before the for-loop to avoid potential weirdness like:

```python
d = {}
d[1] = 2
for x in some_other_list:
    d[x] = x
```
?

If so, is there an example of a rule where we do something similar, I can take inspiration from? I assume it involves scanning the parent scope for the binding and then checking that assignment. But unsure what the most efficient way is to get the relative position of that assignment (resolving the range to line number somehow?)

---

_Closed by @qdegraaf on 2023-07-27 15:12_

---
