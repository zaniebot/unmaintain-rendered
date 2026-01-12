```yaml
number: 12809
title: "Avoid parsing joint rule codes as distinct codes in `# noqa`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - suppression
assignees: []
merged: true
base: main
head: charlie/parse
created_at: 2024-08-11T23:07:25Z
updated_at: 2024-11-02T20:34:20Z
url: https://github.com/astral-sh/ruff/pull/12809
synced_at: 2026-01-12T15:55:42Z
```

# Avoid parsing joint rule codes as distinct codes in `# noqa`

---

_@charliermarsh_

## Summary

We should enable warnings for unsupported codes, but this at least fixes the parsing for `# noqa: F401F841`.

Closes https://github.com/astral-sh/ruff/issues/12808.


---

_Label `bug` added by @charliermarsh on 2024-08-11 23:07_

---

_Label `suppression` added by @charliermarsh on 2024-08-11 23:07_

---

_Comment by @InSyncWithFoo on 2024-08-11 23:20_

[`is_alphanumeric()`](https://doc.rust-lang.org/std/primitive.char.html#method.is_alphanumeric) also allows non-ASCII-digits and letters:

```py
# noqa: A三
```

Did you mean to use [`is_ascii_alphanumeric()`](https://doc.rust-lang.org/std/primitive.char.html#method.is_ascii_alphanumeric) instead?


---

_Comment by @charliermarsh on 2024-08-11 23:22_

Seems reasonable to exclude non-ASCII.

---

_Comment by @codspeed-hq[bot] on 2024-08-11 23:28_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/parse)

### Merging #12809 will **not alter performance**

<sub>Comparing <code>charlie/parse</code> (c2e9746) with <code>main</code> (f837428)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @InSyncWithFoo on 2024-08-11 23:31_

On that same note, [`is_whitespace()`](https://doc.rust-lang.org/std/primitive.char.html#method.is_whitespace) and [`trim_start()` / `trim_end()`](https://doc.rust-lang.org/std/primitive.str.html#method.trim) also handle Unicode whitespace. They should be replaced with [`is_ascii_whitespace()`](https://doc.rust-lang.org/std/primitive.char.html#method.is_ascii_whitespace) and [`trim_ascii_start()` / `trim_ascii_end()`](https://doc.rust-lang.org/std/primitive.str.html#method.trim_ascii), accordingly.

"ASCII whitespace" is [defined](https://doc.rust-lang.org/std/primitive.u8.html#method.is_ascii_whitespace) as "space, tab, LF, FF, CR". In our case, since Python comments don't allow line breaks, that boils down to just spaces and tabs.

---

_Comment by @github-actions[bot] on 2024-08-11 23:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/229c6a3e4673db03119c78828e546ebae272ef75/providers/src/airflow/providers/apache/hdfs/sensors/hdfs.py#L45'>providers/src/airflow/providers/apache/hdfs/sensors/hdfs.py:45:37:</a> RUF100 [*] Unused `noqa` directive (unknown: `Ignore`)
+ <a href='https://github.com/apache/airflow/blob/229c6a3e4673db03119c78828e546ebae272ef75/providers/src/airflow/providers/apache/hdfs/sensors/hdfs.py#L50'>providers/src/airflow/providers/apache/hdfs/sensors/hdfs.py:50:38:</a> RUF100 [*] Unused `noqa` directive (unknown: `Ignore`)
+ <a href='https://github.com/apache/airflow/blob/229c6a3e4673db03119c78828e546ebae272ef75/providers/src/airflow/providers/google/cloud/hooks/bigquery.py#L59'>providers/src/airflow/providers/google/cloud/hooks/bigquery.py:59:42:</a> RUF100 [*] Unused `noqa` directive (unknown: `Used`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/229c6a3e4673db03119c78828e546ebae272ef75/providers/src/airflow/providers/apache/hdfs/sensors/hdfs.py#L45'>providers/src/airflow/providers/apache/hdfs/sensors/hdfs.py:45:37:</a> RUF100 [*] Unused `noqa` directive (unknown: `Ignore`)
+ <a href='https://github.com/apache/airflow/blob/229c6a3e4673db03119c78828e546ebae272ef75/providers/src/airflow/providers/apache/hdfs/sensors/hdfs.py#L50'>providers/src/airflow/providers/apache/hdfs/sensors/hdfs.py:50:38:</a> RUF100 [*] Unused `noqa` directive (unknown: `Ignore`)
+ <a href='https://github.com/apache/airflow/blob/229c6a3e4673db03119c78828e546ebae272ef75/providers/src/airflow/providers/google/cloud/hooks/bigquery.py#L59'>providers/src/airflow/providers/google/cloud/hooks/bigquery.py:59:42:</a> RUF100 [*] Unused `noqa` directive (unknown: `Used`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-08-11 23:41_

But Python comments can contain non-ASCII whitespace? We already have an `is_python_whitespace` that respects the spec for tokens: https://docs.python.org/3/reference/lexical_analysis.html#whitespace-between-tokens

---

_Comment by @InSyncWithFoo on 2024-08-11 23:57_

> But Python comments can contain non-ASCII whitespace?

Fair enough, though I still don't see why someone would want to use non-ASCII whitespace in the `noqa` part of a comment. A user might want to write an explanation in their native language in the same comment, sure, but `noqa` and the rule codes are never localized.

---

_Comment by @InSyncWithFoo on 2024-08-12 03:42_

One more thing: `ParsedFileExemption::try_extract()` [currently treats](https://github.com/astral-sh/ruff/blob/feba5031dca5c5ad1c9b77c2003b2fee738bba93/crates/ruff_linter/src/noqa.rs#L475) `A001 , B002` (whitespace before comma) and `A001,,B002` (empty slot) as if `B002` doesn't exist. Is this difference in behaviour intended?

---

_@dhruvmanila reviewed on 2024-08-12 03:58_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/noqa.rs`:1202 on 2024-08-12 03:58_

I think we should show a warning in this case but still parse the remaining codes (if that's possible currently). This would be similar to how we _recover_ from a missing element in list parsing: https://play.ruff.rs/308ab356-9355-43f3-99b7-12c7c2de7334.

---

_@dhruvmanila approved on 2024-08-12 04:58_

---

_Merged by @charliermarsh on 2024-11-02 20:25_

---

_Closed by @charliermarsh on 2024-11-02 20:25_

---

_Branch deleted on 2024-11-02 20:25_

---
