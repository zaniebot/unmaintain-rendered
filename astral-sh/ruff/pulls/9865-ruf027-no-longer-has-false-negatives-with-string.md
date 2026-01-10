```yaml
number: 9865
title: RUF027 no longer has false negatives with string literals inside of method calls
type: pull_request
state: merged
author: snowsignal
labels:
  - rule
assignees: []
merged: true
base: main
head: jane/linter/RUF027/method-call-fix
created_at: 2024-02-06T21:12:38Z
updated_at: 2024-02-08T15:00:21Z
url: https://github.com/astral-sh/ruff/pull/9865
synced_at: 2026-01-10T22:57:09Z
```

# RUF027 no longer has false negatives with string literals inside of method calls

---

_Pull request opened by @snowsignal on 2024-02-06 21:12_

Fixes #9857.

## Summary

Statements like `logging.info("Today it is: {day}")` will no longer be ignored by RUF027. As before, statements like `"Today it is: {day}".format(day="Tuesday")` will continue to be ignored.

## Test Plan

The snapshot tests were expanded to include new cases. Additionally, the snapshot tests have been split in two to separate positive cases from negative cases.

---

_Label `rule` added by @snowsignal on 2024-02-06 21:12_

---

_Renamed from "RUF027 no longer has false negatives with string literals inside of method calls." to "RUF027 no longer has false negatives with string literals inside of method calls" by @snowsignal on 2024-02-06 21:12_

---

_Review requested from @MichaReiser by @snowsignal on 2024-02-06 21:13_

---

_Comment by @github-actions[bot] on 2024-02-06 21:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+8 -0 violations, +0 -0 fixes in 3 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_response.py#L372'>tests/core/nlg/test_response.py:372:9:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/shared/core/test_slot_mappings.py#L72'>tests/shared/core/test_slot_mappings.py:72:9:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/shared/core/training_data/story_writer/test_yaml_story_writer.py#L201'>tests/shared/core/training_data/story_writer/test_yaml_story_writer.py:201:9:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/test_model_training.py#L922'>tests/test_model_training.py:922:9:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/6f32374fd1add436635c2ea5a6da86ac127c5182/ibis/expr/types/generic.py#L752'>ibis/expr/types/generic.py:752:37:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/96e2ed11f0795e44a3b72a1d6c10087746acfd9c/rotkehlchen/db/dbhandler.py#L2800'>rotkehlchen/db/dbhandler.py:2800:23:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/rotki/rotki/blob/96e2ed11f0795e44a3b72a1d6c10087746acfd9c/rotkehlchen/exchanges/bitpanda.py#L275'>rotkehlchen/exchanges/bitpanda.py:275:47:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/rotki/rotki/blob/96e2ed11f0795e44a3b72a1d6c10087746acfd9c/rotkehlchen/exchanges/kraken.py#L419'>rotkehlchen/exchanges/kraken.py:419:21:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF027 | 8 | 8 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-02-06 21:30_

The Rasa checks look like false positives (hard to avoid...), the Ibis check looks like a true positive (I think), the Pandas check looks like a false positive (maybe avoidable?), the Rotki checks look like they might be true positives.

---

_Comment by @charliermarsh on 2024-02-06 21:30_

Perhaps we could avoid the Pandas false positive by also ignoring calls with positional arguments that use the exact same name...

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:122 on 2024-02-06 23:00_

What if we have `"{test}".attribute.chaining.call()"` Should this be excluded too or is this handled by the `value if last_expr == Some(value)`? If so, it might be worth adding some comments explaining the cases these branches handle

---

_@MichaReiser approved on 2024-02-06 23:01_

---

_@snowsignal reviewed on 2024-02-07 16:21_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:122 on 2024-02-07 16:21_

> Should this be excluded too or is this handled by the value if `last_expr == Some(value)`?

I just tested this, and it is handled by that case! I'll add some clarification though, that's a good idea.

---

_Comment by @snowsignal on 2024-02-07 20:48_

@charliermarsh I added some logic to ignore literals with matching regular arguments in addition to keyword arguments.

---

_Comment by @codspeed-hq[bot] on 2024-02-07 20:54_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/jane/linter/RUF027/method-call-fix)

### Merging #9865 will **improve performances by 11.53%**

<sub>Comparing <code>jane/linter/RUF027/method-call-fix</code> (787ae4e) with <code>main</code> (daae28e)</sub>



### Summary

`⚡ 10` improvements
`✅ 20` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `jane/linter/RUF027/method-call-fix` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 20.6 ms | 18.9 ms | +9.19% |
| ⚡ | `linter/all-rules[large/dataset.py]` | 92.5 ms | 82.9 ms | +11.53% |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 3.1 ms | 2.9 ms | +5.7% |
| ⚡ | `linter/all-with-preview-rules[numpy/ctypeslib.py]` | 23.3 ms | 21.5 ms | +8.31% |
| ⚡ | `linter/all-with-preview-rules[pydantic/types.py]` | 51 ms | 47 ms | +8.66% |
| ⚡ | `linter/all-with-preview-rules[large/dataset.py]` | 105.1 ms | 95.3 ms | +10.23% |
| ⚡ | `linter/all-with-preview-rules[unicode/pypinyin.py]` | 11.8 ms | 11.2 ms | +5.21% |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 2.8 ms | 2.6 ms | +6.35% |
| ⚡ | `linter/all-rules[unicode/pypinyin.py]` | 10.8 ms | 10.2 ms | +5.8% |
| ⚡ | `linter/all-rules[pydantic/types.py]` | 43.9 ms | 39.6 ms | +10.71% |


---

_Merged by @snowsignal on 2024-02-08 15:00_

---

_Closed by @snowsignal on 2024-02-08 15:00_

---

_Branch deleted on 2024-02-08 15:00_

---
