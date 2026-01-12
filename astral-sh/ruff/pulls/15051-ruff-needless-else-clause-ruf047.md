```yaml
number: 15051
title: "[`ruff`] Needless `else` clause (`RUF047`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: RUF047
created_at: 2024-12-19T01:30:38Z
updated_at: 2025-01-21T12:30:36Z
url: https://github.com/astral-sh/ruff/pull/15051
synced_at: 2026-01-12T15:55:50Z
```

# [`ruff`] Needless `else` clause (`RUF047`)

---

_@InSyncWithFoo_

## Summary

Partially addresses #13929.

## Test Plan

`cargo nextest run` and `cargo insta test`.

---

_Comment by @github-actions[bot] on 2024-12-19 01:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+6 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/41b151e7dde473ec445f9f78fb4b8db826c368fc/providers/src/airflow/providers/google/cloud/operators/bigquery.py#L1638'>providers/src/airflow/providers/google/cloud/operators/bigquery.py:1638:9:</a> RUF047 [*] Empty `else` clause
+ <a href='https://github.com/apache/airflow/blob/41b151e7dde473ec445f9f78fb4b8db826c368fc/providers/tests/fab/auth_manager/api_endpoints/test_asset_endpoint.py#L38'>providers/tests/fab/auth_manager/api_endpoints/test_asset_endpoint.py:38:5:</a> RUF047 [*] Empty `else` clause
+ <a href='https://github.com/apache/airflow/blob/41b151e7dde473ec445f9f78fb4b8db826c368fc/providers/tests/fab/auth_manager/api_endpoints/test_dag_run_endpoint.py#L44'>providers/tests/fab/auth_manager/api_endpoints/test_dag_run_endpoint.py:44:5:</a> RUF047 [*] Empty `else` clause
+ <a href='https://github.com/apache/airflow/blob/41b151e7dde473ec445f9f78fb4b8db826c368fc/tests/models/test_taskinstance.py#L2388'>tests/models/test_taskinstance.py:2388:17:</a> RUF047 [*] Empty `else` clause
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L791'>lnbits/core/crud.py:791:5:</a> RUF047 [*] Empty `else` clause
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L800'>lnbits/core/crud.py:800:5:</a> RUF047 [*] Empty `else` clause
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF047 | 6 | 6 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2024-12-19 07:27_

---

_Label `preview` added by @MichaReiser on 2024-12-19 07:27_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/needless_else.rs`:14 on 2024-12-19 07:29_

```suggestion
/// Such an else clause does nothing and can be removed.
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/needless_else.rs`:39 on 2024-12-19 07:30_

```suggestion
        "Remove the `else` clause".to_string()
```

---

_Assigned to @MichaReiser by @MichaReiser on 2024-12-19 08:20_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/mod.rs`:429 on 2024-12-19 08:53_

The needless-else rule has no `mode.is_preview` checks. We can add it to the normal `rules` test, which simplifies promoting them to stable.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__preview__RUF047_RUF047_if.py.snap`:73 on 2024-12-19 08:54_

We should not suggest this fix because the comment belongs to the `else` clause

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/RUF047_if.py`:60 on 2024-12-19 08:55_

We should detect that this `else` branch is useless because the comment belongs to the if block

---

_@MichaReiser requested changes on 2024-12-19 08:58_

Thanks.

I refactored your rule to remove some duplication and simplified it to only detect single `pass` or `...` statements. PIE790 detects bodies containing both a `...` and a `pass` statement. 

I further extended the test cases and there's one case where we fail to detect a useless else because of the comment and one case where we marked the else as useless when we should not. We have to take the indent of the comments into consideration.  See https://github.com/astral-sh/ruff/blob/9f3a38d408f473df7a6b3574c5d1389bef303dd5/crates/ruff_python_formatter/src/comments/placement.rs#L552 and https://github.com/astral-sh/ruff/blob/9f3a38d408f473df7a6b3574c5d1389bef303dd5/crates/ruff_python_formatter/src/comments/placement.rs#L641

Maybe we can come up with a simpler heuristic for this specific case?

---

_Comment by @InSyncWithFoo on 2024-12-19 17:29_

@MichaReiser If indentation is to be taken into consideration, should this be reported or not?

```py
if test:
    _4_spaces()
  # 2 spaces
else:
    ...
```

What about this?

```py
if test:
	_1_tab()  # These two lines would align if tabs are displayed as 1 character wide.
 # 1 space
else:
	...

# Same question, but for 2, 3, 4 spaces?
```

---

_Comment by @MichaReiser on 2024-12-20 08:11_

We should implement it so that it's consistent with the formatter. Your examples get formatted to

```py
if test:
    _1_tab()  # These two lines would align if tabs are displayed as 1 character wide.
    # 1 space
else:
    ...

# Same question, but for 2, 3, 4 spaces?

if test:
    _4_spaces()
# 2 spaces
else:
    ...
```



---

_Comment by @MichaReiser on 2025-01-03 09:36_

@InSyncWithFoo do you plan to follow up on this PR?

---

_Comment by @InSyncWithFoo on 2025-01-03 21:26_

@MichaReiser Yes.

---

_Comment by @InSyncWithFoo on 2025-01-20 04:52_

Took me long enough. I think I got most of it, save for this one:

```python
if a:
	b()
else:
	...
	# comment
```

I have no idea how to detect that comment: it's not included within `else_range`. I thought about manual tokens processing, but then I wouldn't know when to stop, because there might not be a next statement at all. Iterating all comments is a lot more expensive than necessary, so that's not a good choice either.

---

_Comment by @MichaReiser on 2025-01-20 10:18_

> I have no idea how to detect that comment: it's not included within else_range. I thought about manual tokens processing, but then I wouldn't know when to stop, because there might not be a next statement at all. Iterating all comments is a lot more expensive than necessary, so that's not a good choice either.

I would use the tokens and start at the end of the `else_range` and skip all tokens until it finds the first new line token. From there, keep taking all tokens until you find the first non-trivia token that isn't a comment. For all comments in that range, test if the indent of the comment is larger than the indent of the `else` header. If so -> it belongs to the else body. You can stop as soon as you've found the first comment that doesn't belong to the else body.

---

_@MichaReiser approved on 2025-01-21 08:16_

Nice

---

_Merged by @MichaReiser on 2025-01-21 08:21_

---

_Closed by @MichaReiser on 2025-01-21 08:21_

---

_Branch deleted on 2025-01-21 12:30_

---
