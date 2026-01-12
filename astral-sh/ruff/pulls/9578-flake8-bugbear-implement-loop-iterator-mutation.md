```yaml
number: 9578
title: "[`flake8-bugbear`] Implement `loop-iterator-mutation` (`B909`)"
type: pull_request
state: merged
author: mimre25
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-flake8-bugbear-b038
created_at: 2024-01-19T15:12:02Z
updated_at: 2024-04-11T19:57:58Z
url: https://github.com/astral-sh/ruff/pull/9578
synced_at: 2026-01-12T15:55:29Z
```

# [`flake8-bugbear`] Implement `loop-iterator-mutation` (`B909`)

---

_@mimre25_

## Summary
This PR adds the implementation for the current [flake8-bugbear](https://github.com/PyCQA/flake8-bugbear)'s B038 rule. 
The B038 rule checks for mutation of loop iterators in the body of a for loop and alerts when found.

Rational: 
Editing the loop iterator can lead to undesired behavior and is probably a bug in most cases.

Closes #9511.

Note there will be a second iteration of B038 implemented in `flake8-bugbear` soon, and this PR currently only implements the weakest form of the rule.
I'd be happy to also implement the further improvements to B038 here in ruff :slightly_smiling_face: 
See https://github.com/PyCQA/flake8-bugbear/issues/454 for more information on the planned improvements.

## Test Plan
Re-using the same test file that I've used for `flake8-bugbear`, which is included in this PR (look for the `B038.py` file).


Note: this is my first time using `rust` (beside `rustlings`) - I'd be very happy about thorough feedback on what I could've done better :slightly_smiling_face:  - Bring it on :grinning:  



---

_Review requested from @charliermarsh by @charliermarsh on 2024-01-19 16:15_

---

_Label `rule` added by @charliermarsh on 2024-01-19 16:16_

---

_Label `preview` added by @charliermarsh on 2024-01-19 16:16_

---

_Comment by @charliermarsh on 2024-01-19 16:16_

Awesome, thank you! Excited to review, I'll give it a close read per request :)

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:35 on 2024-01-21 15:18_

Nit: can we rename to `LoopIteratorMutation`? Gramatically we want the names to work in a sentence like "allow [rule name]", as in "allow loop iterator mutation".

---

_@charliermarsh reviewed on 2024-01-21 15:18_

---

_@charliermarsh reviewed on 2024-01-21 15:19_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:107 on 2024-01-21 15:19_

Nit: should be able to omit the `{}` after `LoopIteratorMutated`.

---

_@charliermarsh reviewed on 2024-01-21 15:20_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:95 on 2024-01-21 15:20_

We should remove this -- if it's an actual invariant, we should find a way to enforce it with the type system! And if it's actually an error, we should log it with `error!` or similar.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:111 on 2024-01-21 15:22_

In Rust, you almost always want `&str` over `&String`. There's more detail on it here: https://doc.rust-lang.org/nightly/book/ch04-03-slices.html. But one way to think of why it's _useful_ is... if you have `String`, you can convert to `&String` or `&str` (a reference to a string). But if you have `&str`, there's no way to convert to `&String` without allocating.

---

_@charliermarsh reviewed on 2024-01-21 15:22_

---

_@charliermarsh reviewed on 2024-01-21 15:23_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:112 on 2024-01-21 15:23_

I'd suggest just making this `Vec<TextRange>`. Using `Box<dyn ...>` means we need to allocate that data on the heap, and that we need to use dynamic dispatch on methods calls, both of which have (small) costs.

---

_@charliermarsh reviewed on 2024-01-21 15:24_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:14 on 2024-01-21 15:24_

We typically write this kind of thing as a method to enable the compiler to optimize it... Grep for `is_feature_name` as an example of what that looks like.

---

_@charliermarsh reviewed on 2024-01-21 15:25_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:71 on 2024-01-21 15:25_

This feels off to me... Can you give me some more context on what this function is intended to do?

---

_@charliermarsh reviewed on 2024-01-21 15:25_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:68 on 2024-01-21 15:25_

Does this mean that `foo().bar` and `foo.bar` would be considered identical?

---

_@charliermarsh reviewed on 2024-01-21 15:26_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:44 on 2024-01-21 15:26_

You may want `collect_call_path` for this, we have an abstraction for this exact thing, but it stores structured components (like a vector of the dot-separated pieces) rather than a `String`.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:156 on 2024-01-21 15:27_

Do we need this, if we're already look at `Stmt::Delete` above?

Separately, what about `ExprContext::Store`?

---

_@charliermarsh reviewed on 2024-01-21 15:27_

---

_Comment by @charliermarsh on 2024-01-21 15:27_

Thank you! Left some comments -- feel free to ask questions.

---

_@mimre25 reviewed on 2024-01-25 11:56_

---

_Review comment by @mimre25 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:68 on 2024-01-25 11:56_

I tried out `collect_call_path` and `compose_call_path` as you've suggested below, and both of them return `None` for `a().foo`.

Not 100% sure how I should go about this - should this be the case?

Also wondering, would you say the following should be caught?:

```py
for bar in a().foo:
  del a().foo
```


---

_@mimre25 reviewed on 2024-01-25 11:58_

---

_Review comment by @mimre25 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:71 on 2024-01-25 11:58_

I think this is just the base case of the recursion.

Probably gonna remove this function anyway in favor of `compose_call_path`.

---

_@mimre25 reviewed on 2024-01-25 12:00_

---

_Review comment by @mimre25 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:111 on 2024-01-25 12:00_

Thank you for that - finally understand `str` vs `String` :slightly_smiling_face:  

---

_@mimre25 reviewed on 2024-01-25 12:00_

---

_Review comment by @mimre25 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:112 on 2024-01-25 12:00_

I wanted to do that and forgot about it :sweat_smile: 

---

_@mimre25 reviewed on 2024-01-25 12:01_

---

_Review comment by @mimre25 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:14 on 2024-01-25 12:01_

Oh cool - I thought anyway that this seems to be not very rust-like.

Guess I've spent too much time in python :grimacing: 

---

_@mimre25 reviewed on 2024-01-25 13:31_

---

_Review comment by @mimre25 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:95 on 2024-01-25 13:31_

How wold you go about ensuring this with the type system?

---

_@mimre25 reviewed on 2024-01-25 13:32_

---

_Review comment by @mimre25 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:156 on 2024-01-25 13:32_

We will need `ExprContext::Store`, and quite some more additions (see https://github.com/PyCQA/flake8-bugbear/issues/454), but I don't want to pollute the current review with this for your ease. 

I'd add all these improvements later on, once this PR would be ready.

---

_Comment by @mimre25 on 2024-01-25 13:37_

Do you have any suggestion on how to set up the work environment?

In general I think `rust`'s incremental builds are a good idea, but damn they eat up like 10+gb for this project :grimacing: 

---

_@charliermarsh reviewed on 2024-01-25 14:39_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:68 on 2024-01-25 14:39_

I would say that should not be caught, since we can't know that `a()` returns the same object.

---

_Comment by @charliermarsh on 2024-01-25 15:03_

Awesome, I should be able to review and merge this today!

---

_Review comment by @trag1c on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:95 on 2024-01-25 15:24_

I think you should use the [`unreachable!()`](https://doc.rust-lang.org/std/macro.unreachable.html) macro.

---

_@trag1c reviewed on 2024-01-25 15:24_

---

_@mimre25 reviewed on 2024-01-25 17:04_

---

_Review comment by @mimre25 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:68 on 2024-01-25 17:04_

ok, then I think I can use `compose_call_path` - I was just worrying that it returns `None` whenever there is `a()` in the call path.

---

_Comment by @mimre25 on 2024-01-25 17:07_

@charliermarsh please hold off from merging, I still want to remote the `to_name_str` function and use `collect_call_path` instead.

Also, in `flake8-bugbear` there is currently the motion going on to make this an optional rule (disabled per default) as there are some false positives stemming from the python standard library - See the discussion in https://github.com/PyCQA/flake8-bugbear/issues/455

---

_Comment by @charliermarsh on 2024-01-25 17:22_

@mimre25 - Roger that!

---

_Converted to draft by @charliermarsh on 2024-01-25 17:30_

---

_Comment by @charliermarsh on 2024-01-25 17:30_

@mimre25 - I'm going to convert to draft for now just to help manage our review workflow.

---

_Comment by @MichaReiser on 2024-04-08 07:00_

I close because it is stale. Please re-open if you plan to follow up on the remaining work. 

---

_Closed by @MichaReiser on 2024-04-08 07:00_

---

_Comment by @mimre25 on 2024-04-08 14:16_

Sorry for leaving this stale for so long. 

I was quite busy and had planned to finish this up in the next couple of weeks. 

---

_Reopened by @MichaReiser on 2024-04-08 16:07_

---

_Review comment by @mimre25 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutated.rs`:44 on 2024-04-11 15:11_

Could it be that the function was removed or renamed? I can't find it anymore.

---

_@mimre25 reviewed on 2024-04-11 15:11_

---

_Comment by @mimre25 on 2024-04-11 15:15_

Alright, I finally got around to finish this.

This is now on parity with flake8-bugbear's B909 (it was renamed). 

Main new features in the new commits are described in https://github.com/PyCQA/flake8-bugbear/issues/454.
Further, the rule now allows mutations before an unconditional break.

---

_Marked ready for review by @mimre25 on 2024-04-11 15:16_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-11 15:29_

---

_Comment by @charliermarsh on 2024-04-11 15:30_

Thanks! I'll take a look.

---

_Review comment by @trag1c on `crates/ruff_linter/resources/test/fixtures/flake8_bugbear/B909.py`:3 on 2024-04-11 16:25_

Assuming a typo :D
```suggestion
B909 - on lines 11, 25, 26, 40, 46
```

---

_@trag1c reviewed on 2024-04-11 16:25_

---

_Renamed from "feat(rules): Implement flake8-bugbear B038 rule" to "[`flake8-bugbear`] Implement `B909`" by @charliermarsh on 2024-04-11 17:46_

---

_Renamed from "[`flake8-bugbear`] Implement `B909`" to "[`flake8-bugbear`] Implement `loop-iterator-mutation` (`B909`)" by @charliermarsh on 2024-04-11 18:19_

---

_@charliermarsh approved on 2024-04-11 18:26_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutation.rs`:182 on 2024-04-11 18:27_

All these `&Box<Expr>` were changed to `&Expr`.

---

_@charliermarsh reviewed on 2024-04-11 18:27_

---

_@charliermarsh reviewed on 2024-04-11 18:27_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutation.rs`:161 on 2024-04-11 18:27_

All these `&Vec<Expr>` were changed to `&[Expr]`. You almost always want the latter -- it's a lot more flexible, because any reference to a vector can be passed in, but so can other kinds of slices.

---

_Comment by @github-actions[bot] on 2024-04-11 19:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
Cannot overwrite a value (at line 26, column 2)
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -0 violations, +0 -0 fixes in 3 projects; 1 project error; 40 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/05ba268d05787bfbec6b1f5eefde2c90c64bf6b5/airflow/models/taskinstance.py#L3758'>airflow/models/taskinstance.py:3758:17:</a> B909 Mutation to loop iterable `new_dict` during iteration
+ <a href='https://github.com/apache/airflow/blob/05ba268d05787bfbec6b1f5eefde2c90c64bf6b5/airflow/providers/openlineage/sqlparser.py#L353'>airflow/providers/openlineage/sqlparser.py:353:13:</a> B909 Mutation to loop iterable `tables` during iteration
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/mypy/blob/e2fc1f28935806ca04b18fab277217f583b51594/mypy/build.py#L3200'>mypy/build.py:3200:21:</a> B909 Mutation to loop iterable `new` during iteration
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build/blob/c97af94e3a5d4ba2228cd5425e1290e5bb4ee30a/tests/pytest_helpers.py#L49'>tests/pytest_helpers.py:49:13:</a> B909 Mutation to loop iterable `expected_content` during iteration
+ <a href='https://github.com/scikit-build/scikit-build/blob/c97af94e3a5d4ba2228cd5425e1290e5bb4ee30a/tests/pytest_helpers.py#L92'>tests/pytest_helpers.py:92:13:</a> B909 Mutation to loop iterable `expected_content` during iteration
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
Cannot overwrite a value (at line 26, column 2)
```

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B909 | 5 | 5 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-04-11 19:16_

There are some false positives in here. For example:

```python
for auth_method in authentication_methods_dict:
    authentication_methods_dict[auth_method] = True
```

That's probably the most common?

---

_Merged by @charliermarsh on 2024-04-11 19:52_

---

_Closed by @charliermarsh on 2024-04-11 19:52_

---
