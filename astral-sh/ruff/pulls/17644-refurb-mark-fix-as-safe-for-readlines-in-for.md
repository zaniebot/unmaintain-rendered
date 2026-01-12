```yaml
number: 17644
title: "[`refurb`] Mark fix as safe for `readlines-in-for` (`FURB129`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: furb-129-safe
created_at: 2025-04-26T15:22:35Z
updated_at: 2025-04-28T14:39:55Z
url: https://github.com/astral-sh/ruff/pull/17644
synced_at: 2026-01-12T15:56:02Z
```

# [`refurb`] Mark fix as safe for `readlines-in-for` (`FURB129`)

---

_@dylwil3_

This PR promotes the fix applicability of [readlines-in-for (FURB129)](https://docs.astral.sh/ruff/rules/readlines-in-for/#readlines-in-for-furb129) to always safe.

In the original PR (https://github.com/astral-sh/ruff/pull/9880), the author marked the rule as unsafe because Ruff's type inference couldn't quite guarantee that we had an `IOBase` object in hand. Some false positives were recorded in the test fixture. However, before the PR was merged, Charlie added the necessary type inference and the false positives went away.

According to the [Python documentation](https://docs.python.org/3/library/io.html#io.IOBase), I believe this fix is safe for any proper implementation of `IOBase`:

>[IOBase](https://docs.python.org/3/library/io.html#io.IOBase) (and its subclasses) supports the iterator protocol, meaning that an [IOBase](https://docs.python.org/3/library/io.html#io.IOBase) object can be iterated over yielding the lines in a stream. Lines are defined slightly differently depending on whether the stream is a binary stream (yielding bytes), or a text stream (yielding character strings). See [readline()](https://docs.python.org/3/library/io.html#io.IOBase.readline) below.

and then in the [documentation for `readlines`](https://docs.python.org/3/library/io.html#io.IOBase.readlines):

>Read and return a list of lines from the stream. hint can be specified to control the number of lines read: no more lines will be read if the total size (in bytes/characters) of all lines so far exceeds hint. [...]
>Note that it’s already possible to iterate on file objects using for line in file: ... without calling file.readlines().

I believe that a careful reading of our [versioning policy](https://docs.astral.sh/ruff/versioning/#version-changes) requires that this change be deferred to a minor release - but please correct me if I'm wrong!

---

_Label `breaking` added by @dylwil3 on 2025-04-26 15:22_

---

_Label `fixes` added by @dylwil3 on 2025-04-26 15:22_

---

_Label `do-not-merge` added by @dylwil3 on 2025-04-26 15:22_

---

_Added to milestone `v0.12` by @dylwil3 on 2025-04-26 15:22_

---

_Renamed from "[`refurb`] Mark fix as safe for readlines-in-for (FURB129)" to "[`refurb`] Mark fix as safe for `readlines-in-for` (`FURB129`)" by @dylwil3 on 2025-04-26 15:27_

---

_Comment by @github-actions[bot] on 2025-04-26 15:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +10 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +10 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/7a4e726bc63079fca21c4ecf31ae5d22460e67d5/dev/airflow_perf/sql_queries.py#L145'>dev/airflow_perf/sql_queries.py:145:41:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
+ <a href='https://github.com/apache/airflow/blob/7a4e726bc63079fca21c4ecf31ae5d22460e67d5/dev/airflow_perf/sql_queries.py#L145'>dev/airflow_perf/sql_queries.py:145:41:</a> FURB129 [*] Instead of calling `readlines()`, iterate over file object directly
- <a href='https://github.com/apache/airflow/blob/7a4e726bc63079fca21c4ecf31ae5d22460e67d5/dev/breeze/src/airflow_breeze/global_constants.py#L553'>dev/breeze/src/airflow_breeze/global_constants.py:553:21:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
+ <a href='https://github.com/apache/airflow/blob/7a4e726bc63079fca21c4ecf31ae5d22460e67d5/dev/breeze/src/airflow_breeze/global_constants.py#L553'>dev/breeze/src/airflow_breeze/global_constants.py:553:21:</a> FURB129 [*] Instead of calling `readlines()`, iterate over file object directly
- <a href='https://github.com/apache/airflow/blob/7a4e726bc63079fca21c4ecf31ae5d22460e67d5/devel-common/src/sphinx_exts/redirects.py#L44'>devel-common/src/sphinx_exts/redirects.py:44:21:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
+ <a href='https://github.com/apache/airflow/blob/7a4e726bc63079fca21c4ecf31ae5d22460e67d5/devel-common/src/sphinx_exts/redirects.py#L44'>devel-common/src/sphinx_exts/redirects.py:44:21:</a> FURB129 [*] Instead of calling `readlines()`, iterate over file object directly
- <a href='https://github.com/apache/airflow/blob/7a4e726bc63079fca21c4ecf31ae5d22460e67d5/providers/google/tests/system/google/cloud/gcs/resources/transform_script.py#L26'>providers/google/tests/system/google/cloud/gcs/resources/transform_script.py:26:39:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
+ <a href='https://github.com/apache/airflow/blob/7a4e726bc63079fca21c4ecf31ae5d22460e67d5/providers/google/tests/system/google/cloud/gcs/resources/transform_script.py#L26'>providers/google/tests/system/google/cloud/gcs/resources/transform_script.py:26:39:</a> FURB129 [*] Instead of calling `readlines()`, iterate over file object directly
- <a href='https://github.com/apache/airflow/blob/7a4e726bc63079fca21c4ecf31ae5d22460e67d5/scripts/ci/pre_commit/newsfragments.py#L36'>scripts/ci/pre_commit/newsfragments.py:36:43:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
+ <a href='https://github.com/apache/airflow/blob/7a4e726bc63079fca21c4ecf31ae5d22460e67d5/scripts/ci/pre_commit/newsfragments.py#L36'>scripts/ci/pre_commit/newsfragments.py:36:43:</a> FURB129 [*] Instead of calling `readlines()`, iterate over file object directly
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB129 | 10 | 0 | 0 | 10 | 0 |

</p>
</details>




---

_@MichaReiser approved on 2025-04-28 07:35_

This sounds reasonable to me. Yes, I think this needs to be gated behind preview

---

_Label `breaking` removed by @dylwil3 on 2025-04-28 14:15_

---

_Label `do-not-merge` removed by @dylwil3 on 2025-04-28 14:15_

---

_Label `preview` added by @dylwil3 on 2025-04-28 14:15_

---

_Removed from milestone `v0.12` by @dylwil3 on 2025-04-28 14:15_

---

_Merged by @dylwil3 on 2025-04-28 14:39_

---

_Closed by @dylwil3 on 2025-04-28 14:39_

---
