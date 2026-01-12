```yaml
number: 16719
title: "[`perflint`] Implement fix for `manual-dict-comprehension` (`PERF403`)"
type: pull_request
state: merged
author: w0nder1ng
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: perf403_fix
created_at: 2025-03-14T03:53:39Z
updated_at: 2025-04-19T15:17:25Z
url: https://github.com/astral-sh/ruff/pull/16719
synced_at: 2026-01-12T15:55:56Z
```

# [`perflint`] Implement fix for `manual-dict-comprehension` (`PERF403`)

---

_@w0nder1ng_

## Summary

This change adds an auto-fix for manual dict comprehensions. It also copies many of the improvements from #13919 (and associated PRs fixing issues with it), and moves some of the utility functions from `manual_list_comprehension.rs` into a separate `helpers.rs` to be used in both. 

## Test Plan

I added a preview test case to showcase the new fix and added a test case in `PERF403.py` to make sure lines with semicolons function. I didn't yet make similar tests to the ones I added earlier to `PERF401.py`, but the logic is the same, so it might be good to add those to make sure they work.


---

_Comment by @github-actions[bot] on 2025-03-14 04:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+6 -8 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/d5e8b4b9301f9aef08f56fc12ef46c3cbf40a191/providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py#L141'>providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py:141:17:</a> PERF403 Use `dict.update` instead of a for-loop
- <a href='https://github.com/apache/airflow/blob/d5e8b4b9301f9aef08f56fc12ef46c3cbf40a191/providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py#L141'>providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py:141:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/d5e8b4b9301f9aef08f56fc12ef46c3cbf40a191/providers/exasol/src/airflow/providers/exasol/hooks/exasol.py#L70'>providers/exasol/src/airflow/providers/exasol/hooks/exasol.py:70:17:</a> PERF403 Use `dict.update` instead of a for-loop
- <a href='https://github.com/apache/airflow/blob/d5e8b4b9301f9aef08f56fc12ef46c3cbf40a191/providers/exasol/src/airflow/providers/exasol/hooks/exasol.py#L70'>providers/exasol/src/airflow/providers/exasol/hooks/exasol.py:70:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/d5e8b4b9301f9aef08f56fc12ef46c3cbf40a191/providers/mysql/src/airflow/providers/mysql/hooks/mysql.py#L191'>providers/mysql/src/airflow/providers/mysql/hooks/mysql.py:191:17:</a> PERF403 Use `dict.update` instead of a for-loop
- <a href='https://github.com/apache/airflow/blob/d5e8b4b9301f9aef08f56fc12ef46c3cbf40a191/providers/mysql/src/airflow/providers/mysql/hooks/mysql.py#L191'>providers/mysql/src/airflow/providers/mysql/hooks/mysql.py:191:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/d5e8b4b9301f9aef08f56fc12ef46c3cbf40a191/providers/postgres/src/airflow/providers/postgres/hooks/postgres.py#L171'>providers/postgres/src/airflow/providers/postgres/hooks/postgres.py:171:17:</a> PERF403 Use `dict.update` instead of a for-loop
- <a href='https://github.com/apache/airflow/blob/d5e8b4b9301f9aef08f56fc12ef46c3cbf40a191/providers/postgres/src/airflow/providers/postgres/hooks/postgres.py#L171'>providers/postgres/src/airflow/providers/postgres/hooks/postgres.py:171:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/26ff734ef94658f622eef494555863c9f9f68aaa/superset/migrations/versions/2021-04-12_12-38_fc3a3a8ff221_migrate_filter_sets_to_new_format.py#L122'>superset/migrations/versions/2021-04-12_12-38_fc3a3a8ff221_migrate_filter_sets_to_new_format.py:122:21:</a> PERF403 Use `dict.update` instead of a for-loop
- <a href='https://github.com/apache/superset/blob/26ff734ef94658f622eef494555863c9f9f68aaa/superset/migrations/versions/2021-04-12_12-38_fc3a3a8ff221_migrate_filter_sets_to_new_format.py#L122'>superset/migrations/versions/2021-04-12_12-38_fc3a3a8ff221_migrate_filter_sets_to_new_format.py:122:21:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/superset/blob/26ff734ef94658f622eef494555863c9f9f68aaa/superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py#L795'>superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py:795:21:</a> PERF403 Use `dict.update` instead of a for-loop
- <a href='https://github.com/apache/superset/blob/26ff734ef94658f622eef494555863c9f9f68aaa/superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py#L795'>superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py:795:21:</a> PERF403 Use a dictionary comprehension instead of a for-loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/5fef9793dd23867e7b227a1df7aa60a283f6204e/pandas/tests/arrays/sparse/test_accessor.py#L247'>pandas/tests/arrays/sparse/test_accessor.py:247:30:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
- <a href='https://github.com/pandas-dev/pandas/blob/5fef9793dd23867e7b227a1df7aa60a283f6204e/pandas/tests/arrays/sparse/test_accessor.py#L96'>pandas/tests/arrays/sparse/test_accessor.py:96:50:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PERF403 | 12 | 6 | 6 | 0 | 0 |
| RUF043 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Label `fixes` added by @MichaReiser on 2025-03-14 08:31_

---

_Comment by @Skylion007 on 2025-03-15 15:36_

Slight nit: had a situation where it didn't simplify an `annotated dict = dict()` builtin to a single line comprehension. Still kept a sepereate call to the `dict()` builtin function followed by update. Manually fixed that.

```python
    node_to_step: dict[BaseSchedulerNode, int] = dict()
    for step, node in enumerate(nodes):
        node_to_step[node] = step
```
got transformed to
```python
     node_to_step: dict[BaseSchedulerNode, int] = dict()
     node_to_step.update(...)
```

---

_Comment by @w0nder1ng on 2025-03-16 17:33_

I had forgotten about using a function call to make a dict, since it's not typically something I do in my own code. I've now modified the "is empty dict" check to allow builtin function calls for both PERF403 and PERF401.

---

_Comment by @Skylion007 on 2025-03-17 16:19_

Title is wrong, it's PERF403, right?

---

_Renamed from "[perflint] implement quick-fix for manual-dict-comprehension (PERF404)" to "[perflint] implement quick-fix for manual-dict-comprehension (PERF403)" by @w0nder1ng on 2025-03-17 16:41_

---

_Comment by @w0nder1ng on 2025-03-23 23:17_

Do you know why the tests aren't running? The test jobs in my fork of the repo are timing out while waiting for a runner.

---

_Comment by @ntBre on 2025-03-24 14:25_

I think the most recent commits came in while CI was disabled. You can try pushing another (possibly empty) commit, or closing and reopening the PR to retrigger CI.

---

_Comment by @Skylion007 on 2025-04-02 13:54_

@ntBre CI is run now.

---

_Comment by @ntBre on 2025-04-03 20:48_

Thanks for the ping, I'll try to review this tomorrow.

---

_Assigned to @ntBre by @ntBre on 2025-04-03 20:48_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/helpers.rs`:1 on 2025-04-04 20:34_

Just confirming: these were moved over from `manual_list_comprehension.rs` without any changes? (besides converting them to functions) I still looked at them pretty closely, but I didn't review them as closely as I would brand-new code.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:3 on 2025-04-04 20:36_

nit: I would  merge these two imports:
```suggestion
use crate::rules::perflint::helpers::{comment_strings_in_range, statement_deletion_range};
```

and also move the `crate` imports back down to a separate block like `crate::checkers::ast::Checker` was before.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:5 on 2025-04-04 20:37_

```suggestion
use ruff_diagnostics::{Diagnostic, Edit, Fix, FixAvailability, Violation};
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:11 on 2025-04-04 20:38_

nit: same as above, merge with the other `ruff_python_semantic` import.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:157 on 2025-04-04 20:49_

I think this is probably a reasonable `expect` call, but we might as well just return early if it's `None`, just in case.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:163 on 2025-04-04 20:50_

tiny nit: I think `other_reference.start() > for_stmt.end()` would be slightly more intuitive, like the comment above.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:190 on 2025-04-04 20:53_

Is this the same as inside the `tuple.iter().any` call above? It might be worth factoring out a small helper function here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:8 on 2025-04-04 20:55_

I'd also like an empty line here before the docs.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:260 on 2025-04-04 20:59_

I think you can remove this block:

```suggestion
    let assignment_in_same_statement = binding.source.is_some_and(|binding_source| {
        let for_loop_parent = checker.semantic().current_statement_parent_id();
        let binding_parent = checker.semantic().parent_statement_id(binding_source);
        for_loop_parent == binding_parent
    });
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:268 on 2025-04-04 21:01_

```suggestion
    // If the binding gets used in between the assignment and the for loop, a dict comprehension is no longer safe
```

or just `comprehension` maybe?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:281 on 2025-04-04 21:03_

I thought we already checked the end of the `for` loop vs any references to the variable above? Maybe I'm missing something.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:301 on 2025-04-04 21:05_

How hard/intrusive would it be to check this earlier? There's a lot of new code above this that would be nice to skip if we're not going to use the fix. However, it's also nice not to have to change much when stabilizing the rule, so if it's more than moving this check up a bit, don't worry about it.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:303 on 2025-04-04 21:07_

I'd lean towards passing these as separate `key` and `value` arguments. If this was to avoid `clippy::too_many_arguments`, it's fine to add an `#[allow(...)]` here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:84 on 2025-04-04 21:08_

Does this need to be `&mut`? With `report_diagnostic`, I think `&` should be fine.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:352 on 2025-04-04 21:11_

```suggestion
    // {... for i in (a, b)}
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:357 on 2025-04-04 21:16_

This is slightly shorter but maybe not more readable. Up to you.
```suggestion
    let iter_str = if let Expr::Tuple(ast::ExprTuple {
        parenthesized: false,
        ..
    }) = &*for_stmt.iter
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:397 on 2025-04-04 21:20_

This is all a nice touch. I don't usually worry about formatting when generating a fix :smile: 

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:402 on 2025-04-04 21:21_

It looks like these two are used in both arms of the `match` and could be hoisted out.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/snapshots/ruff_linter__rules__perflint__tests__preview__PERF403_PERF403.py.snap`:174 on 2025-04-04 21:25_

This is kind of a strange order. Is this how the list version works? It's already an unsafe fix, so it's pretty reasonable to drop the comments instead of trying to sort them out, if it's difficult to do. We just need to mention it in the `## Fix safety` docs.

---

_@ntBre reviewed on 2025-04-04 21:31_

Thanks for all of this work! It looks good, and most of my inline comments are just stylistic nits.

For tests, I think I'd like to see ~1 test for each of the (very helpful) comments that document some constraint, if that's not too hard. Is that what you had in mind in the PR summary?

---

_@w0nder1ng reviewed on 2025-04-07 01:35_

---

_Review comment by @w0nder1ng on `crates/ruff_linter/src/rules/perflint/helpers.rs`:1 on 2025-04-07 01:35_

Yes, there are no changes. I just extracted them into functions so I could reuse the code in `manual_dict_comprehension.rs`.

---

_@w0nder1ng reviewed on 2025-04-07 02:01_

---

_Review comment by @w0nder1ng on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:281 on 2025-04-07 02:01_

This checks whether the result dictionary is accessed between its assignment and the for loop. I was trying to get across that it's not safe to transform this:
```python
res = {}
print(len(res))
values = {"a": 1, "b": 2}
for k, v in values.items():
    res[k] = v
```
into this:
```python
print(len(res))  # NameError
values = {"a": 1, "b": 2}
res = {k: v for k, v in values.items()}
```
In this case, it needs to be `.update` instead, since the binding statement needs to stay put

---

_@w0nder1ng reviewed on 2025-04-07 02:11_

---

_Review comment by @w0nder1ng on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:402 on 2025-04-07 02:11_

The indentation depends on whether the comments are empty, and the relevant comments depend on the type of fix, so I think indentation has to stay in both. 

---

_@w0nder1ng reviewed on 2025-04-07 02:23_

---

_Review comment by @w0nder1ng on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:157 on 2025-04-07 02:23_

I was thinking that if it doesn't exist, then there is some flaw in my code, so it'd be better to find out with a panic than to silently ignore the logic error. Is there a better way to report an invariant being broken without a panic?

---

_Comment by @w0nder1ng on 2025-04-07 02:55_

@ntBre I think I've addressed all of your feedback, so please let me know if you need anything else changed. 

Also, do you know why the `configuration.md` output test is failing? I don't think I touched any files that affect it.

---

_Comment by @ntBre on 2025-04-07 12:54_

> Also, do you know why the `configuration.md` output test is failing? I don't think I touched any files that affect it.

At the very top of the error message it says 

> Error: docs/configuration.md changed, please run `cargo dev generate-all`:

I think adding a fix probably changed the docs slightly, and you just need to run `cargo dev generate-all` to fix it. Some of the diffs look kind of weird, though, so you may also need to merge `main` or something, but try `generate-all` first.

---

_@ntBre reviewed on 2025-04-07 13:04_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:157 on 2025-04-07 13:04_

I might be going overboard here, but I'm pretty wary of user-facing panics, at least when they're easily avoidable. There are a couple of other options here:
1. `debug_assert` that the `find` result is `Some` to panic only in dev builds
2. Log an error, probably with `debug!`, so that the user can see it with `--verbose` if they encounter a surprising false negative
3. There's also a `warn_user_once` macro, but I think that's for higher-priority issues whereas this is more for debugging

I think I'd prefer (1) or (2) but no strong preference between them. `debug!` is probably slightly neater in a `let-else` with `return` but `debug_assert` gives the stronger guarantee, at least while testing.

---

_@ntBre reviewed on 2025-04-07 13:06_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:402 on 2025-04-07 13:06_

Oh I see now, thanks.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:302 on 2025-04-07 13:12_

You can use `Diagnostic::with_fix` here

---

_@ntBre approved on 2025-04-07 13:14_

Thanks! I'd still like to avoid the `expect` calls, and I added one more suggestion for `Diagnostic::with_fix` but this looks good to me otherwise, once the tests are passing.

---

_Comment by @w0nder1ng on 2025-04-07 18:33_

Turns out I just needed to merge with main. Did you also want the `.expect()` in `convert_to_dict_comprehension` removed?

---

_Comment by @ntBre on 2025-04-07 21:06_

> Turns out I just needed to merge with main. Did you also want the `.expect()` in `convert_to_dict_comprehension` removed?

Yes please!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:143 on 2025-04-07 21:09_

You could include your previous `expect` message as the `debug_assert` message, if you want.
```suggestion
        debug_assert!(target_binding.is_some(), "for-loop target binding must exist");
```

---

_@ntBre approved on 2025-04-07 21:09_

---

_Comment by @w0nder1ng on 2025-04-15 23:33_

@ntBre let me know if any other changes are necessary

---

_@ntBre approved on 2025-04-16 16:54_

---

_Renamed from "[perflint] implement quick-fix for manual-dict-comprehension (PERF403)" to "[`perflint`] Implement fix for `manual-dict-comprehension` (`PERF403`)" by @ntBre on 2025-04-16 16:55_

---

_@ntBre reviewed on 2025-04-16 17:08_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:282 on 2025-04-16 17:08_

Sorry for not noticing this earlier (or for bringing it up again if we already discussed it), but should this be included here? I think these changes (and the corresponding ones on the dict side) are causing the differences in the ecosystem check, and I would kind of like to limit this PR strictly to the autofix.

I would have at least expected some PERF401 snapshots to change with this, since it's obviously affecting ecosystem packages.

---

_Review comment by @Skylion007 on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:282 on 2025-04-16 20:29_

We can split it off in a different PR, but it's an ecosystem bug we noticed when testing.

---

_@Skylion007 reviewed on 2025-04-16 20:29_

---

_Label `preview` added by @ntBre on 2025-04-18 17:09_

---

_Comment by @ntBre on 2025-04-18 17:10_

Thanks, I thought we had to revert the `Call` check in the `dict` too, but that's in the new preview part anyway.

---

_Merged by @ntBre on 2025-04-18 17:10_

---

_Closed by @ntBre on 2025-04-18 17:10_

---

_Comment by @Skylion007 on 2025-04-19 15:17_

@w0ndering Mind opening a new PR for that ecosystem fix to list Calls as well?

---
