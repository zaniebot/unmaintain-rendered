```yaml
number: 9174
title: "[`pylint`] Implement `empty-comment` (`PLR2044`)"
type: pull_request
state: merged
author: mdbernard
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: PLR2044-empty-comment
created_at: 2023-12-18T03:49:44Z
updated_at: 2023-12-29T03:05:04Z
url: https://github.com/astral-sh/ruff/pull/9174
synced_at: 2026-01-12T15:55:28Z
```

# [`pylint`] Implement `empty-comment` (`PLR2044`)

---

_@mdbernard_

## Summary

Part of #970.

This adds Pylint's [R0244 empty_comment](https://pylint.pycqa.org/en/latest/user_guide/messages/refactor/empty-comment.html) lint as well as an always-safe fix.

## Test Plan

The included snapshot verifies the following:

- A line containing only a non-empty comment is not changed
- A line containing leading whitespace before a non-empty comment is not changed
- A line containing only an empty comment has its content deleted
- A line containing only leading whitespace before an empty comment has its content deleted
- A line containing only leading and trailing whitespace on an empty comment has its content deleted
- A line containing trailing whitespace after a non-empty comment is not changed
- A line containing only a single newline character (i.e. a blank line) is not changed
- A line containing code followed by a non-empty comment is not changed
- A line containing code followed by an empty comment has its content deleted after the last non-whitespace character
- Lines containing code and no comments are not changed
- Empty comment lines within block comments are ignored
- Empty comments within triple-quoted sections are ignored

## Comparison to Pylint

Running Ruff and Pylint 3.0.3 with Python 3.12.0 against the `empty_comment.py` file added in this PR, we see the following:

* Identical behavior:
    * empty_comment.py:3:0: R2044: Line with empty comment (empty-comment)
    * empty_comment.py:4:0: R2044: Line with empty comment (empty-comment)
    * empty_comment.py:5:0: R2044: Line with empty comment (empty-comment)
    * empty_comment.py:18:0: R2044: Line with empty comment (empty-comment)

* Differing behavior:
    * Pylint doesn't ignore empty comments in block comments commonly used for visual spacing; I decided these were fine in this implementation since many projects use these and likely do not want them removed.
        * empty_comment.py:28:0: R2044: Line with empty comment (empty-comment)
    * Pylint detects "empty comments" within the triple-quoted section at the bottom of the file, which is arguably a bug in the Pylint implementation since these are not truly comments. These are ignored by this implementation.
        * empty_comment.py:37:0: R2044: Line with empty comment (empty-comment)
        * empty_comment.py:38:0: R2044: Line with empty comment (empty-comment)
        * empty_comment.py:39:0: R2044: Line with empty comment (empty-comment)

---

_Comment by @github-actions[bot] on 2023-12-18 04:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+27 -2 violations, +0 -0 fixes in 6 projects; 35 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/docs/exts/docroles.py#L20'>docs/exts/docroles.py:20:1:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/docs/exts/docroles.py#L21'>docs/exts/docroles.py:21:1:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/tests/providers/common/sql/hooks/test_sql.py#L18'>tests/providers/common/sql/hooks/test_sql.py:18:1:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/tests/providers/databricks/hooks/test_databricks_sql.py#L18'>tests/providers/databricks/hooks/test_databricks_sql.py:18:1:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/tests/providers/exasol/hooks/test_sql.py#L18'>tests/providers/exasol/hooks/test_sql.py:18:1:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/tests/providers/google/suite/transfers/test_gcs_to_gdrive.py#L117'>tests/providers/google/suite/transfers/test_gcs_to_gdrive.py:117:5:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/tests/providers/snowflake/hooks/test_sql.py#L18'>tests/providers/snowflake/hooks/test_sql.py:18:1:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/tests/system/providers/google/cloud/storage_transfer/example_cloud_storage_transfer_service_aws.py#L121'>tests/system/providers/google/cloud/storage_transfer/example_cloud_storage_transfer_service_aws.py:121:5:</a> PLR2044 [*] Line with empty comment
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/034e027b4010a709d101a035fa41813c63bc790e/samcli/local/docker/lambda_container.py#L149'>samcli/local/docker/lambda_container.py:149:9:</a> PLR2044 [*] Line with empty comment
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/b86e0023787809eafb8153084ad66b72c9586703/pymilvus/client/abstract.py#L114'>pymilvus/client/abstract.py:114:9:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/milvus-io/pymilvus/blob/b86e0023787809eafb8153084ad66b72c9586703/pymilvus/client/abstract.py#L18'>pymilvus/client/abstract.py:18:9:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/milvus-io/pymilvus/blob/b86e0023787809eafb8153084ad66b72c9586703/pymilvus/client/abstract.py#L99'>pymilvus/client/abstract.py:99:9:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/milvus-io/pymilvus/blob/b86e0023787809eafb8153084ad66b72c9586703/pymilvus/client/types.py#L133'>pymilvus/client/types.py:133:5:</a> PLR2044 [*] Line with empty comment
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/37975ebafae0ded16951a23d027ba63d23dca349/pandas/tests/extension/decimal/array.py#L175'>pandas/tests/extension/decimal/array.py:175:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/pandas-dev/pandas/blob/37975ebafae0ded16951a23d027ba63d23dca349/pandas/tests/extension/decimal/array.py#L176'>pandas/tests/extension/decimal/array.py:176:9:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/pandas-dev/pandas/blob/37975ebafae0ded16951a23d027ba63d23dca349/pandas/tests/extension/decimal/array.py#L233'>pandas/tests/extension/decimal/array.py:233:9:</a> PLR6301 Method `_formatter` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/37975ebafae0ded16951a23d027ba63d23dca349/pandas/tests/extension/decimal/array.py#L234'>pandas/tests/extension/decimal/array.py:234:9:</a> PLR6301 Method `_formatter` could be a function, class method, or static method
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/d786be1da4a115513ade47701e95c2b05333fcb1/rotkehlchen/tests/db/test_db.py#L752'>rotkehlchen/tests/db/test_db.py:752:48:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/rotki/rotki/blob/d786be1da4a115513ade47701e95c2b05333fcb1/rotkehlchen/tests/db/test_db.py#L757'>rotkehlchen/tests/db/test_db.py:757:48:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/rotki/rotki/blob/d786be1da4a115513ade47701e95c2b05333fcb1/rotkehlchen/tests/db/test_db.py#L795'>rotkehlchen/tests/db/test_db.py:795:52:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/rotki/rotki/blob/d786be1da4a115513ade47701e95c2b05333fcb1/rotkehlchen/tests/db/test_db.py#L797'>rotkehlchen/tests/db/test_db.py:797:52:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/rotki/rotki/blob/d786be1da4a115513ade47701e95c2b05333fcb1/rotkehlchen/tests/db/test_db.py#L801'>rotkehlchen/tests/db/test_db.py:801:52:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/rotki/rotki/blob/d786be1da4a115513ade47701e95c2b05333fcb1/rotkehlchen/tests/db/test_db.py#L834'>rotkehlchen/tests/db/test_db.py:834:52:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/rotki/rotki/blob/d786be1da4a115513ade47701e95c2b05333fcb1/rotkehlchen/tests/db/test_db.py#L837'>rotkehlchen/tests/db/test_db.py:837:52:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/rotki/rotki/blob/d786be1da4a115513ade47701e95c2b05333fcb1/rotkehlchen/tests/db/test_db.py#L843'>rotkehlchen/tests/db/test_db.py:843:52:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/rotki/rotki/blob/d786be1da4a115513ade47701e95c2b05333fcb1/rotkehlchen/tests/db/test_db.py#L844'>rotkehlchen/tests/db/test_db.py:844:52:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/rotki/rotki/blob/d786be1da4a115513ade47701e95c2b05333fcb1/rotkehlchen/tests/db/test_db.py#L845'>rotkehlchen/tests/db/test_db.py:845:52:</a> PLR2044 [*] Line with empty comment
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/baf6c68ab8d6861b17179d4087933d2601b010f9/zerver/lib/export.py#L842'>zerver/lib/export.py:842:5:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/zulip/zulip/blob/baf6c68ab8d6861b17179d4087933d2601b010f9/zerver/lib/export.py#L867'>zerver/lib/export.py:867:5:</a> PLR2044 [*] Line with empty comment
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR2044 | 25 | 25 | 0 | 0 | 0 |
| PLC0415 | 2 | 1 | 1 | 0 | 0 |
| PLR6301 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @mdbernard on 2023-12-18 04:32_

Looks like the ecosystem checks found some edge cases not currently covered:

- "empty comment" lines within block comments, which add vertical whitespace for clarity, especially common as header comments for files
- lines containing a trailing `#` character in multi-line comments, which should not be considered empty comments

---

_Comment by @charliermarsh on 2023-12-18 04:36_

Nice, thanks for kicking this off! Agreed on the edge cases described above.

---

_Converted to draft by @mdbernard on 2023-12-18 06:06_

---

_@mdbernard reviewed on 2023-12-18 06:15_

---

_Review comment by @mdbernard on `crates/ruff_linter/resources/test/fixtures/pylint/empty_comment.py`:1 on 2023-12-18 06:15_

- [x]  Add test for "empty comment" lines in multi-line strings/comments
- [x]   Add test for "empty comment" lines in block comments

---

_Comment by @mdbernard on 2023-12-20 06:47_

- [x] Account for comment style with trailing `#` at end of comment, e.g. https://github.com/rotki/rotki/blob/63302ca6d67ab928fd6cac7b6137c41df416416c/rotkehlchen/chain/ethereum/modules/l2/loopring.py#L527:

```python
# -- Methods following the EthereumModule interface -- #
```

---

_Comment by @mdbernard on 2023-12-20 06:51_

- [x] Account for comment style with multiple `#` symbols on a single line (tying in with the above comment, probably best to just lint when a comment consists of only a single `#` symbol followed by only whitespace through the end of the line.

---

_@mdbernard reviewed on 2023-12-27 06:31_

- [x] Rebase

---

_Marked ready for review by @mdbernard on 2023-12-28 02:38_

---

_Review requested from @charliermarsh by @charliermarsh on 2023-12-28 14:46_

---

_@charliermarsh reviewed on 2023-12-28 16:39_

This looks great, I really like what I'm seeing in the ecosystem checks too -- thank you!

I'm going to explore making this a token-based rule rather than a physical line-based rule, just because we want to avoid using physical lines when possible, but it'll be a mostly mechanical change...

---

_Comment by @charliermarsh on 2023-12-28 16:41_

> Pylint doesn't ignore empty comments in block comments commonly used for visual spacing; I decided these were fine in this implementation since many projects use these and likely do not want them removed.

I agree, we shouldn't flag these.

> Pylint detects "empty comments" within the triple-quoted section at the bottom of the file, which is arguably a bug in the Pylint implementation since these are not truly comments. These are ignored by this implementation.

I agree, flagging empty comments within a multi-line string seems like a bug to me.


---

_Comment by @mdbernard on 2023-12-28 19:43_

- [ ] Noting that I might've missed an edge case where two empty comments exist on consecutive lines forming a "block" comment.

---

_Comment by @mdbernard on 2023-12-28 19:44_

> we want to avoid using physical lines when possible

Out of curiosity, why?

---

_Comment by @charliermarsh on 2023-12-29 02:14_

> Out of curiosity, why?

Primarily because physical line-based approaches tend to be slower, since we need to run the rule over _every_ line. This rule is actually a good example: we already _know_ the locations of all comments in the file (via `CommentRanges`), so iterating over every line, then checking if the line intersects with the comment ranges, is wasteful as compared to merely iterating over the comment ranges directly. In general, we often have information we've already computed and/or can exploit to limit the search space vs. iterating over the physical lines.

---

_Label `rule` added by @charliermarsh on 2023-12-29 02:34_

---

_Label `preview` added by @charliermarsh on 2023-12-29 02:34_

---

_Renamed from "[pylint] add PLR2044 empty_comment" to "[`pylint`] Implement `empty-comment` (`PLR2044`)" by @charliermarsh on 2023-12-29 02:34_

---

_@charliermarsh approved on 2023-12-29 02:49_

This is great work! I ended up moving to the token checker, and added support for the case in which we have multiple consecutive empty comments. 

---

_Merged by @charliermarsh on 2023-12-29 02:53_

---

_Closed by @charliermarsh on 2023-12-29 02:53_

---
