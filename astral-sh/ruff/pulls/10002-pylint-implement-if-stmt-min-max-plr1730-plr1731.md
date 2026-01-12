```yaml
number: 10002
title: "[`pylint`] Implement `if-stmt-min-max` (`PLR1730`, `PLR1731`)"
type: pull_request
state: merged
author: tibor-reiss
labels:
  - rule
  - accepted
  - preview
assignees: []
merged: true
base: main
head: add-PLR1730-PLR1731
created_at: 2024-02-15T15:32:50Z
updated_at: 2024-04-11T14:54:07Z
url: https://github.com/astral-sh/ruff/pull/10002
synced_at: 2026-01-12T15:55:30Z
```

# [`pylint`] Implement `if-stmt-min-max` (`PLR1730`, `PLR1731`)

---

_@tibor-reiss_

Add rule [consider-using-min-builtin (R1730)](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/consider-using-min-builtin.html) and [consider-using-max-builtin (R1731)](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/consider-using-max-builtin.html)

See https://github.com/astral-sh/ruff/issues/970 for rules

Test Plan: `cargo test`

---

_Comment by @github-actions[bot] on 2024-02-15 15:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+77 -70 violations, +0 -0 fixes in 4 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/cli/commands/kubernetes_command.py#L84'>airflow/cli/commands/kubernetes_command.py:84:5:</a> PLR1730 [*] Replace `if` statement with `min_pending_minutes = min(min_pending_minutes, 5)`
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/models/taskinstance.py#L2167'>airflow/models/taskinstance.py:2167:13:</a> PLR1730 [*] Replace `if` statement with `min_backoff = min(min_backoff, 1)`
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/tests/cluster_policies/__init__.py#L103'>tests/cluster_policies/__init__.py:103:5:</a> PLR1730 [*] Replace `if` statement with `max` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/722d2b5bca2946b0bbefdeb909b0891d89c2615f/pymilvus/bulk_writer/buffer.py#L234'>pymilvus/bulk_writer/buffer.py:234:13:</a> PLR1730 [*] Replace `if` statement with `min` call
+ <a href='https://github.com/milvus-io/pymilvus/blob/722d2b5bca2946b0bbefdeb909b0891d89c2615f/pymilvus/bulk_writer/buffer.py#L236'>pymilvus/bulk_writer/buffer.py:236:13:</a> PLR1730 [*] Replace `if` statement with `max` call
+ <a href='https://github.com/milvus-io/pymilvus/blob/722d2b5bca2946b0bbefdeb909b0891d89c2615f/pymilvus/orm/iterator.py#L54'>pymilvus/orm/iterator.py:54:9:</a> PLR1730 [*] Replace `if` statement with `max` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+70 -70 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1000'>pandas/tests/arithmetic/test_numeric.py:1000:9:</a> PLR6301 Method `test_frame_operators_none_to_nan` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1002'>pandas/tests/arithmetic/test_numeric.py:1002:9:</a> PLR6301 Method `test_frame_operators_none_to_nan` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1005'>pandas/tests/arithmetic/test_numeric.py:1005:9:</a> PLR6301 Method `test_frame_operators_empty_like` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1007'>pandas/tests/arithmetic/test_numeric.py:1007:9:</a> PLR6301 Method `test_frame_operators_empty_like` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1021'>pandas/tests/arithmetic/test_numeric.py:1021:9:</a> PLR6301 Method `test_series_operators_arithmetic` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1023'>pandas/tests/arithmetic/test_numeric.py:1023:9:</a> PLR6301 Method `test_series_operators_arithmetic` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1034'>pandas/tests/arithmetic/test_numeric.py:1034:9:</a> PLR6301 Method `test_series_operators_compare` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1036'>pandas/tests/arithmetic/test_numeric.py:1036:9:</a> PLR6301 Method `test_series_operators_compare` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1049'>pandas/tests/arithmetic/test_numeric.py:1049:9:</a> PLR6301 Method `test_divmod` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1051'>pandas/tests/arithmetic/test_numeric.py:1051:9:</a> PLR6301 Method `test_divmod` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1077'>pandas/tests/arithmetic/test_numeric.py:1077:9:</a> PLR6301 Method `test_series_divmod_zero` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1079'>pandas/tests/arithmetic/test_numeric.py:1079:9:</a> PLR6301 Method `test_series_divmod_zero` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1103'>pandas/tests/arithmetic/test_numeric.py:1103:9:</a> PLR6301 Method `test_ufunc_compat` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1105'>pandas/tests/arithmetic/test_numeric.py:1105:9:</a> PLR6301 Method `test_ufunc_compat` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1118'>pandas/tests/arithmetic/test_numeric.py:1118:9:</a> PLR6301 Method `test_ufunc_coercions` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1120'>pandas/tests/arithmetic/test_numeric.py:1120:9:</a> PLR6301 Method `test_ufunc_coercions` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1162'>pandas/tests/arithmetic/test_numeric.py:1162:9:</a> PLR6301 Method `test_ufunc_multiple_return_values` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1164'>pandas/tests/arithmetic/test_numeric.py:1164:9:</a> PLR6301 Method `test_ufunc_multiple_return_values` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1173'>pandas/tests/arithmetic/test_numeric.py:1173:9:</a> PLR6301 Method `test_ufunc_at` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1175'>pandas/tests/arithmetic/test_numeric.py:1175:9:</a> PLR6301 Method `test_ufunc_at` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1184'>pandas/tests/arithmetic/test_numeric.py:1184:9:</a> PLR6301 Method `test_numarr_with_dtype_add_nan` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1186'>pandas/tests/arithmetic/test_numeric.py:1186:9:</a> PLR6301 Method `test_numarr_with_dtype_add_nan` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1199'>pandas/tests/arithmetic/test_numeric.py:1199:9:</a> PLR6301 Method `test_numarr_with_dtype_add_int` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1201'>pandas/tests/arithmetic/test_numeric.py:1201:9:</a> PLR6301 Method `test_numarr_with_dtype_add_int` could be a function, class method, or static method
... 113 additional changes omitted for rule PLR6301
+ <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1491'>pandas/tests/arithmetic/test_numeric.py:1491:18:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1493'>pandas/tests/arithmetic/test_numeric.py:1493:18:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1493'>pandas/tests/arithmetic/test_numeric.py:1493:19:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/b8a4691647a8850d681409c5dd35a12726cd94a1/pandas/tests/arithmetic/test_numeric.py#L1495'>pandas/tests/arithmetic/test_numeric.py:1495:19:</a> PLR6201 Use a `set` literal when testing for membership
... 112 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/d4caff8d2df144bf7ffa4026efc6bfb062306c9d/rotkehlchen/exchanges/bybit.py#L403'>rotkehlchen/exchanges/bybit.py:403:13:</a> PLR1730 [*] Replace `if` statement with `lower_ts = min(lower_ts, start_ts)`
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR6301 | 136 | 68 | 68 | 0 | 0 |
| PLR1730 | 7 | 7 | 0 | 0 | 0 |
| PLR6201 | 4 | 2 | 2 | 0 | 0 |

</p>
</details>




---

_Comment by @tibor-reiss on 2024-02-15 16:19_

Hi @charliermarsh, some questions regarding this PR:
- min and max are 2 separate rules in pylint, so I implemented as such with their respective codes - looking at other similar rules this seems to be the standard for now. However, there is minimal difference between the 2 functions min_instead_of_if and max_instead_of_if. So I wonder if it would not be better to merge these 2 rules into one?
~~- is there some simple general way of matching the "value" part in literals, i.e. ignoring the range field? E.g. I could not find any example so far for a simple comparison of two python constants.~~

---

_Comment by @sbrugman on 2024-02-16 12:14_

Another related rule:
https://github.com/dosisod/refurb/blob/master/docs/checks.md#furb136-use-min-max

#1348 implemented as https://docs.astral.sh/ruff/rules/if-expr-min-max/

---

_Comment by @tibor-reiss on 2024-02-17 08:42_

@sbrugman While they look indeed similar, the implementation is quite different: FURB136 is for if expressions (in ruff it is ExprIfExp which is an Expr enum), whereas PLR1730 operates on if statements (in ruff it is StmtIf which is a Stmt enum).

---

_Comment by @sbrugman on 2024-02-17 09:48_

I linked it as reference for the first point: here it is implemented as a single rule. Consistency with the upstream (mapping existing rules) takes precedence over internal consistency (merging rules that are minimally different) I suppose, so just for reference.

Nice work!

---

_Comment by @tibor-reiss on 2024-02-17 16:29_

@sbrugman Thanks for clarifying, I misunderstood your original comment. I added now a commit which merges the 2 rules - I personally also prefer this over the duplication. @charliermarsh to decide if this is acceptable - i.e. dropping PLR1731 and just using PLR1730 for both min and max.

---

_Comment by @sbrugman on 2024-02-17 16:59_

Sorry for the confusion, but I intended to say that the linked rule merged them at one, but since that pylint treats them as two for now the project prefers to match pylint (i.e. two rules). Anyway, indeed let the team decide on that

---

_Comment by @tibor-reiss on 2024-02-17 21:09_

@sbrugman From the linked reference I found out that there is ComparableExpr - code looks a lot cleaner now. So thanks a lot for taking a look!

---

_Label `accepted` added by @MichaReiser on 2024-04-05 10:22_

---

_Comment by @MichaReiser on 2024-04-05 10:23_

The rule itself fits into ruff. It is similar to [if-expr-min-max](https://docs.astral.sh/ruff/rules/if-expr-min-max/) but we have other rules that capture the same problem but differ in the syntax. 

I think we should align the name with `if-expr-min-max` -> `if-stmt-min-max`.

One thing that needs to be decided if this should be a single rule or if it's better to have two rules (to match py lint)

---

_Comment by @charliermarsh on 2024-04-06 16:54_

I'm comfortable with a single rule here. Just seems unlikely that anyone would enable one but not the other, and it better aligns with `if-expr-min-max`. I'll rename and merge.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-06 16:54_

---

_Label `preview` added by @charliermarsh on 2024-04-06 16:54_

---

_@charliermarsh approved on 2024-04-06 17:19_

---

_Renamed from "Implement consider-using-max/min-builtin (PLR1730, PLR1731)" to "[`pylint`] Implement `if-stmt-min-max` (`PLR1730`, `PLR1731`)" by @charliermarsh on 2024-04-06 17:25_

---

_Merged by @charliermarsh on 2024-04-06 17:32_

---

_Closed by @charliermarsh on 2024-04-06 17:32_

---

_Label `rule` added by @dhruvmanila on 2024-04-11 14:54_

---
