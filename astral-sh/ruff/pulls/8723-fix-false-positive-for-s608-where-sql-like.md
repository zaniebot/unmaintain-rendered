```yaml
number: 8723
title: "Fix false positive for `S608` where SQL-like expression follows other text"
type: pull_request
state: closed
author: zanieb
labels:
  - bug
assignees: []
draft: true
base: main
head: zanie/S608
created_at: 2023-11-16T15:49:45Z
updated_at: 2024-08-24T22:16:10Z
url: https://github.com/astral-sh/ruff/pull/8723
synced_at: 2026-01-10T21:38:31Z
```

# Fix false positive for `S608` where SQL-like expression follows other text

---

_Pull request opened by @zanieb on 2023-11-16 15:49_

Closes https://github.com/astral-sh/ruff/issues/8717

Updates the regex to enforce that the SQL keywords are at the start of the string. Unfortunately we are passing "generated expression strings" to the regular expression instead of the inner contents of a string, so I need to allow for all sorts of cruft in front e.g. `f"`, `f'`, `'`, `"`, `\n` (literal), and whitespace.

---

_Label `bug` added by @zanieb on 2023-11-16 15:49_

---

_Review requested from @BurntSushi by @zanieb on 2023-11-16 15:56_

---

_Comment by @github-actions[bot] on 2023-11-16 16:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -11 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/airflow/example_dags/tutorial_objectstorage.py#L118'>airflow/example_dags/tutorial_objectstorage.py:118:22:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/airflow/providers/amazon/aws/transfers/s3_to_redshift.py#L185'>airflow/providers/amazon/aws/transfers/s3_to_redshift.py:185:17:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/airflow/providers/databricks/operators/databricks_sql.py#L323'>airflow/providers/databricks/operators/databricks_sql.py:323:24:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/airflow/utils/db.py#L1197'>airflow/utils/db.py:1197:30:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/airflow/utils/db_cleanup.py#L236'>airflow/utils/db_cleanup.py:236:12:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/tests/providers/amazon/aws/transfers/test_s3_to_redshift.py#L224'>tests/providers/amazon/aws/transfers/test_s3_to_redshift.py:224:23:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/tests/providers/databricks/operators/test_databricks_copy.py#L107'>tests/providers/databricks/operators/test_databricks_copy.py:107:12:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/tests/providers/databricks/operators/test_databricks_copy.py#L65'>tests/providers/databricks/operators/test_databricks_copy.py:65:12:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/tests/providers/databricks/operators/test_databricks_copy.py#L87'>tests/providers/databricks/operators/test_databricks_copy.py:87:12:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/tests/system/providers/google/cloud/bigquery/example_bigquery_queries_async.py#L67'>tests/system/providers/google/cloud/bigquery/example_bigquery_queries_async.py:67:18:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/tests/system/providers/trino/example_trino.py#L64'>tests/system/providers/trino/example_trino.py:64:13:</a> S608 Possible SQL injection vector through string-based query construction
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S608 | 11 | 0 | 11 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -11 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/airflow/example_dags/tutorial_objectstorage.py#L118'>airflow/example_dags/tutorial_objectstorage.py:118:22:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/airflow/providers/amazon/aws/transfers/s3_to_redshift.py#L185'>airflow/providers/amazon/aws/transfers/s3_to_redshift.py:185:17:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/airflow/providers/databricks/operators/databricks_sql.py#L323'>airflow/providers/databricks/operators/databricks_sql.py:323:24:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/airflow/utils/db.py#L1197'>airflow/utils/db.py:1197:30:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/airflow/utils/db_cleanup.py#L236'>airflow/utils/db_cleanup.py:236:12:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/tests/providers/amazon/aws/transfers/test_s3_to_redshift.py#L224'>tests/providers/amazon/aws/transfers/test_s3_to_redshift.py:224:23:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/tests/providers/databricks/operators/test_databricks_copy.py#L107'>tests/providers/databricks/operators/test_databricks_copy.py:107:12:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/tests/providers/databricks/operators/test_databricks_copy.py#L65'>tests/providers/databricks/operators/test_databricks_copy.py:65:12:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/tests/providers/databricks/operators/test_databricks_copy.py#L87'>tests/providers/databricks/operators/test_databricks_copy.py:87:12:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/tests/system/providers/google/cloud/bigquery/example_bigquery_queries_async.py#L67'>tests/system/providers/google/cloud/bigquery/example_bigquery_queries_async.py:67:18:</a> S608 Possible SQL injection vector through string-based query construction
- <a href='https://github.com/apache/airflow/blob/be6e2cd0d42abc1b3099910c91982f31a98f4c3d/tests/system/providers/trino/example_trino.py#L64'>tests/system/providers/trino/example_trino.py:64:13:</a> S608 Possible SQL injection vector through string-based query construction
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S608 | 11 | 0 | 11 | 0 | 0 |

</p>
</details>




---

_Comment by @zanieb on 2023-11-16 16:07_

All of the airflow ecosystem checks are false negatives :(

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:24 on 2023-11-16 16:33_

One possible thing that might help here is to use verbose mode in the regex. That lets you break things apart using insignificant whitespace and even add comments if you want:

```rust
r#"(?ix)
\A
(\"|f\"|\'|f\'|\\|\\n|\s)*
\b
(
  select\s.+\sfrom\s
  |
  delete\s+from\s
  |
  (insert|replace)\s.+\svalues\s
  |
  update\s.+\sset\s
)
"#
```

Or somethinglike that. Dealer's choice about how best to break it up.

Now that I've written out, it makes me wonder about the following:

* Is it intentional to have the escapes in `\"|f\"|\'|f\'|\\|\\n|\s` here?
Namely, since this is a raw string, `f\"` will match `f\"` literally and not
`f"`. Similarly for the other branches.
* While the regex didn't previously have it, is it perhaps desirable to add a
word boundary assertion at the end as well? Although maybe not, since I see
that each of the `select`, `delete`, `insert|replace` and `update` branches
all must end with a `\s`.

---

_@BurntSushi reviewed on 2023-11-16 16:35_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:24 on 2023-11-16 17:17_

I like a verbose regex with comments :)

> Is it intentional to have the escapes

For `\\n`, yes I want to match the literal `\n` not a newline. For `f\"` it looks like the `\` just has no effect? With or without it, we still match f-strings in the test fixture.



---

_@zanieb reviewed on 2023-11-16 17:17_

---

_@zanieb reviewed on 2023-11-16 17:18_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:24 on 2023-11-16 17:18_

Do you have suggestions for handling the false negatives? It looks like the regex would need to get much more complicated. This change may not be worth it as-is.

---

_@BurntSushi reviewed on 2023-11-16 17:32_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:24 on 2023-11-16 17:32_

Derp, yes, `f\"` is equivalent to `f"`. (Older versions of the regex crate would actually reject such escapes as superfluous.)

With respect to FNs and FPs, are you constrained to a single regex? If not, you could combine the status quo with what you have here. You could change the status quo to only match for uppercase things like `SELECT`. That will reduce its false positives but greatly increase the false negatives. Then to decrease false negatives, you could use your second regex here which allows lowercase `select` but only at the start of a string.

It's pretty hokey and probably is an overfitting to our current tests but is perhaps an improvement.

---

_@zanieb reviewed on 2023-11-16 20:02_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:24 on 2023-11-16 20:02_

Hmm... definitely hokey but I was also thinking case sensitivity makes some sense...

I almost would rather have a "case insensitive" setting for this which is off by default...

---

_@dhruvmanila reviewed on 2023-11-20 19:51_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:24 on 2023-11-20 19:51_

For f-strings, we wouldn't need to worry about the prefix and quotes as in https://github.com/astral-sh/ruff/pull/7927/files#diff-649016c25b517ba99e70caf422c95ef9d544e7c806d26e64624c0414777d54f5 I've removed them. That is, for f-strings, we'd do:

```rs
/// Concatenates the contents of an f-string, without the prefix and quotes,
/// and escapes any special characters.
///
/// ## Example
///
/// ```python
/// "foo" f"bar {x}" "baz"
/// ```
///
/// becomes `foobar {x}baz`.
```

---

_Comment by @brondsem on 2023-12-19 16:02_

While this regex is being improved, here's an example that is NOT detected by ruff currently (1.0.8) but is detected by bandit:

```
f"""SELECT
    foo from bar where a={x}"""
```

---

_Comment by @charliermarsh on 2023-12-19 21:14_

Thank you @brondsem :)

---

_Converted to draft by @zanieb on 2024-03-12 18:38_

---

_Comment by @zanieb on 2024-03-12 18:38_

This turned out to be hard to get right. I'd welcome help improving this but it's going to be tough to balance.

---

_Closed by @zanieb on 2024-03-12 18:38_

---

_Comment by @Arexils on 2024-08-24 22:16_

i have this problem :(

S608 Possible SQL injection vector through string-based query construction

`logger.warning(f'The linked role <{1}> has been delete from the guild {2}')`

---
