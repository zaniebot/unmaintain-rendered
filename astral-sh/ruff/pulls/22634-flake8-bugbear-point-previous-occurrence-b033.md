```yaml
number: 22634
title: "[`flake8-bugbear`] point previous occurrence (`B033`)"
type: pull_request
state: open
author: leandrobbraga
labels:
  - rule
assignees: []
base: main
head: feat/improve-b033-error-message
created_at: 2026-01-16T23:18:56Z
updated_at: 2026-01-17T00:22:53Z
url: https://github.com/astral-sh/ruff/pull/22634
synced_at: 2026-01-17T01:09:41Z
```

# [`flake8-bugbear`] point previous occurrence (`B033`)

---

_@leandrobbraga_

Improves B033 linting error by pointing to the previous occurrence of the duplicate item.

# Before
```text
B033 [*] Sets should not contain duplicate item `"value1"`
 --> B033.py:4:35
4 | incorrect_set = {"value1", 23, 5, "value1"}
  |                                   ^^^^^^^^
help: Remove duplicate item
  - incorrect_set = {"value1", 23, 5, "value1"}
4 + incorrect_set = {"value1", 23, 5}
```

# After
```text
B033 [*] Sets should not contain duplicate item `"value1"`
 --> B033.py:4:18
4 | incorrect_set = {"value1", 23, 5, "value1"}
  |                  --------         ^^^^^^^^ duplicated here
  |                  |
  |                  previous occurrence here
help: Remove duplicate item
  - incorrect_set = {"value1", 23, 5, "value1"}
4 + incorrect_set = {"value1", 23, 5}
```

---

_Comment by @astral-sh-bot[bot] on 2026-01-16 23:30_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+6 -6 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/airflow-core/tests/unit/always/test_project_structure.py#L452'>airflow-core/tests/unit/always/test_project_structure.py:452:9:</a> B033 [*] Sets should not contain duplicate item `"airflow.providers.google.cloud.operators.datacatalog.CloudDataCatalogCreateEntryOperator"`
+ <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/airflow-core/tests/unit/always/test_project_structure.py#L452'>airflow-core/tests/unit/always/test_project_structure.py:452:9:</a> B033 [*] Sets should not contain duplicate item `"airflow.providers.google.cloud.operators.datacatalog.CloudDataCatalogCreateEntryOperator"`: duplicated here
- <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/providers/weaviate/tests/unit/weaviate/hooks/test_weaviate.py#L838'>providers/weaviate/tests/unit/weaviate/hooks/test_weaviate.py:838:39:</a> B033 [*] Sets should not contain duplicate item `"uuid2"`
+ <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/providers/weaviate/tests/unit/weaviate/hooks/test_weaviate.py#L838'>providers/weaviate/tests/unit/weaviate/hooks/test_weaviate.py:838:39:</a> B033 [*] Sets should not contain duplicate item `"uuid2"`: duplicated here
- <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py#L971'>shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py:971:40:</a> B033 [*] Sets should not contain duplicate item `"***"`
+ <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py#L971'>shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py:971:40:</a> B033 [*] Sets should not contain duplicate item `"***"`: duplicated here
- <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py#L973'>shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py:973:40:</a> B033 [*] Sets should not contain duplicate item `"***"`
+ <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py#L973'>shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py:973:40:</a> B033 [*] Sets should not contain duplicate item `"***"`: duplicated here
- <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py#L977'>shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py:977:39:</a> B033 [*] Sets should not contain duplicate item `"***"`
+ <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py#L977'>shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py:977:39:</a> B033 [*] Sets should not contain duplicate item `"***"`: duplicated here
- <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py#L979'>shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py:979:39:</a> B033 [*] Sets should not contain duplicate item `"***"`
+ <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py#L979'>shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py:979:39:</a> B033 [*] Sets should not contain duplicate item `"***"`: duplicated here
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B033 | 12 | 6 | 6 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+6 -6 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/airflow-core/tests/unit/always/test_project_structure.py#L452'>airflow-core/tests/unit/always/test_project_structure.py:452:9:</a> B033 [*] Sets should not contain duplicate item `"airflow.providers.google.cloud.operators.datacatalog.CloudDataCatalogCreateEntryOperator"`
+ <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/airflow-core/tests/unit/always/test_project_structure.py#L452'>airflow-core/tests/unit/always/test_project_structure.py:452:9:</a> B033 [*] Sets should not contain duplicate item `"airflow.providers.google.cloud.operators.datacatalog.CloudDataCatalogCreateEntryOperator"`: duplicated here
- <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/providers/weaviate/tests/unit/weaviate/hooks/test_weaviate.py#L838'>providers/weaviate/tests/unit/weaviate/hooks/test_weaviate.py:838:39:</a> B033 [*] Sets should not contain duplicate item `"uuid2"`
+ <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/providers/weaviate/tests/unit/weaviate/hooks/test_weaviate.py#L838'>providers/weaviate/tests/unit/weaviate/hooks/test_weaviate.py:838:39:</a> B033 [*] Sets should not contain duplicate item `"uuid2"`: duplicated here
- <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py#L971'>shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py:971:40:</a> B033 [*] Sets should not contain duplicate item `"***"`
+ <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py#L971'>shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py:971:40:</a> B033 [*] Sets should not contain duplicate item `"***"`: duplicated here
- <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py#L973'>shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py:973:40:</a> B033 [*] Sets should not contain duplicate item `"***"`
+ <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py#L973'>shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py:973:40:</a> B033 [*] Sets should not contain duplicate item `"***"`: duplicated here
- <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py#L977'>shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py:977:39:</a> B033 [*] Sets should not contain duplicate item `"***"`
+ <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py#L977'>shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py:977:39:</a> B033 [*] Sets should not contain duplicate item `"***"`: duplicated here
- <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py#L979'>shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py:979:39:</a> B033 [*] Sets should not contain duplicate item `"***"`
+ <a href='https://github.com/apache/airflow/blob/97ea9fb1c79526fe990d53c8b028158f0cead5e0/shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py#L979'>shared/secrets_masker/tests/secrets_masker/test_secrets_masker.py:979:39:</a> B033 [*] Sets should not contain duplicate item `"***"`: duplicated here
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B033 | 12 | 6 | 6 | 0 | 0 |

</p>
</details>





---

_@amyreese reviewed on 2026-01-16 23:40_

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/flake8_bugbear/rules/duplicate_value.rs`:75 on 2026-01-16 23:40_

```suggestion
                diagnostic.secondary_annotation(format_args!("previous occurrence here"), existing);
                diagnostic.set_primary_message(format_args!("duplicated here"));
```

---

_@amyreese approved on 2026-01-16 23:40_

---

_Review requested from @ntBre by @amyreese on 2026-01-16 23:40_

---

_Label `rule` added by @amyreese on 2026-01-16 23:40_

---

_Comment by @amyreese on 2026-01-16 23:41_

Is there an example showing what would happen if there were three occurrences of the same value in a single set, eg `{1, 1, 1}`?

---

_Comment by @leandrobbraga on 2026-01-16 23:52_

> Is there an example showing what would happen if there were three occurrences of the same value in a single set, eg `{1, 1, 1}`?

It always consider the immediately previous element as the previous occurrence.

```console
$ echo '{1, 1, 1}' | cargo run -p ruff  -- check --select B033 -
B033 [*] Sets should not contain duplicate item `1`
 --> -:1:2
  |
1 | {1, 1, 1}
  |  -  ^ duplicated here
  |  |
  |  previous occurrence here
  |
help: Remove duplicate item

B033 [*] Sets should not contain duplicate item `1`
 --> -:1:5
  |
1 | {1, 1, 1}
  |     -  ^ duplicated here
  |     |
  |     previous occurrence here
  |
help: Remove duplicate item

Found 2 errors.
[*] 2 fixable with the `--fix` option.
```

---

_Renamed from "[`ruff`] point previous occurrence in B033" to "[`flake8-bugbear`] point previous occurrence in (`B033`)" by @leandrobbraga on 2026-01-17 00:12_

---

_Renamed from "[`flake8-bugbear`] point previous occurrence in (`B033`)" to "[`flake8-bugbear`] point previous occurrence (`B033`)" by @leandrobbraga on 2026-01-17 00:12_

---
