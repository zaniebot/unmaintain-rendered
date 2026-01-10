```yaml
number: 18328
title: Correct the warning for Rule E712 in pytest assertion
type: pull_request
state: merged
author: CodeMan62
labels:
  - diagnostics
assignees: []
merged: true
base: main
head: codeman/18272
created_at: 2025-05-26T20:18:17Z
updated_at: 2025-05-28T07:06:39Z
url: https://github.com/astral-sh/ruff/pull/18328
synced_at: 2026-01-10T18:51:02Z
```

# Correct the warning for Rule E712 in pytest assertion

---

_Pull request opened by @CodeMan62 on 2025-05-26 20:18_

## Summary
closes #18272 

## Test Plan

i have run simple tests via `cargo test` and `cargo insta test`
also done review via `cargo insta review`

---

_Comment by @github-actions[bot] on 2025-05-26 20:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+9 -9 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/request_llms/oai_std_model_template.py#L230'>request_llms/oai_std_model_template.py:230:41:</a> E712 Avoid equality comparisons to `False`; use `if not reasoning:` for false checks
+ <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/request_llms/oai_std_model_template.py#L230'>request_llms/oai_std_model_template.py:230:41:</a> E712 Avoid equality comparisons to `False`; use `not reasoning:` for false checks
- <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/request_llms/oai_std_model_template.py#L376'>request_llms/oai_std_model_template.py:376:41:</a> E712 Avoid equality comparisons to `False`; use `if not reasoning:` for false checks
+ <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/request_llms/oai_std_model_template.py#L376'>request_llms/oai_std_model_template.py:376:41:</a> E712 Avoid equality comparisons to `False`; use `not reasoning:` for false checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/qdrant/qdrant-client">qdrant/qdrant-client</a> (+7 -7 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/collections_pb2.py#L806'>qdrant_client/grpc/collections_pb2.py:806:4:</a> E712 Avoid equality comparisons to `False`; use `if not _descriptor._USE_C_DESCRIPTORS:` for false checks
+ <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/collections_pb2.py#L806'>qdrant_client/grpc/collections_pb2.py:806:4:</a> E712 Avoid equality comparisons to `False`; use `not _descriptor._USE_C_DESCRIPTORS:` for false checks
- <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/collections_service_pb2.py#L23'>qdrant_client/grpc/collections_service_pb2.py:23:4:</a> E712 Avoid equality comparisons to `False`; use `if not _descriptor._USE_C_DESCRIPTORS:` for false checks
+ <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/collections_service_pb2.py#L23'>qdrant_client/grpc/collections_service_pb2.py:23:4:</a> E712 Avoid equality comparisons to `False`; use `not _descriptor._USE_C_DESCRIPTORS:` for false checks
- <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/json_with_int_pb2.py#L58'>qdrant_client/grpc/json_with_int_pb2.py:58:4:</a> E712 Avoid equality comparisons to `False`; use `if not _descriptor._USE_C_DESCRIPTORS:` for false checks
+ <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/json_with_int_pb2.py#L58'>qdrant_client/grpc/json_with_int_pb2.py:58:4:</a> E712 Avoid equality comparisons to `False`; use `not _descriptor._USE_C_DESCRIPTORS:` for false checks
- <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/points_pb2.py#L1295'>qdrant_client/grpc/points_pb2.py:1295:4:</a> E712 Avoid equality comparisons to `False`; use `if not _descriptor._USE_C_DESCRIPTORS:` for false checks
+ <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/points_pb2.py#L1295'>qdrant_client/grpc/points_pb2.py:1295:4:</a> E712 Avoid equality comparisons to `False`; use `not _descriptor._USE_C_DESCRIPTORS:` for false checks
- <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/points_service_pb2.py#L23'>qdrant_client/grpc/points_service_pb2.py:23:4:</a> E712 Avoid equality comparisons to `False`; use `if not _descriptor._USE_C_DESCRIPTORS:` for false checks
+ <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/points_service_pb2.py#L23'>qdrant_client/grpc/points_service_pb2.py:23:4:</a> E712 Avoid equality comparisons to `False`; use `not _descriptor._USE_C_DESCRIPTORS:` for false checks
- <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/qdrant_pb2.py#L41'>qdrant_client/grpc/qdrant_pb2.py:41:4:</a> E712 Avoid equality comparisons to `False`; use `if not _descriptor._USE_C_DESCRIPTORS:` for false checks
+ <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/qdrant_pb2.py#L41'>qdrant_client/grpc/qdrant_pb2.py:41:4:</a> E712 Avoid equality comparisons to `False`; use `not _descriptor._USE_C_DESCRIPTORS:` for false checks
- <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/snapshots_service_pb2.py#L103'>qdrant_client/grpc/snapshots_service_pb2.py:103:4:</a> E712 Avoid equality comparisons to `False`; use `if not _descriptor._USE_C_DESCRIPTORS:` for false checks
+ <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/snapshots_service_pb2.py#L103'>qdrant_client/grpc/snapshots_service_pb2.py:103:4:</a> E712 Avoid equality comparisons to `False`; use `not _descriptor._USE_C_DESCRIPTORS:` for false checks
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E712 | 18 | 9 | 9 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+9 -9 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/request_llms/oai_std_model_template.py#L230'>request_llms/oai_std_model_template.py:230:41:</a> E712 Avoid equality comparisons to `False`; use `if not reasoning:` for false checks
+ <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/request_llms/oai_std_model_template.py#L230'>request_llms/oai_std_model_template.py:230:41:</a> E712 Avoid equality comparisons to `False`; use `not reasoning:` for false checks
- <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/request_llms/oai_std_model_template.py#L376'>request_llms/oai_std_model_template.py:376:41:</a> E712 Avoid equality comparisons to `False`; use `if not reasoning:` for false checks
+ <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/request_llms/oai_std_model_template.py#L376'>request_llms/oai_std_model_template.py:376:41:</a> E712 Avoid equality comparisons to `False`; use `not reasoning:` for false checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/qdrant/qdrant-client">qdrant/qdrant-client</a> (+7 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/collections_pb2.py#L806'>qdrant_client/grpc/collections_pb2.py:806:4:</a> E712 Avoid equality comparisons to `False`; use `if not _descriptor._USE_C_DESCRIPTORS:` for false checks
+ <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/collections_pb2.py#L806'>qdrant_client/grpc/collections_pb2.py:806:4:</a> E712 Avoid equality comparisons to `False`; use `not _descriptor._USE_C_DESCRIPTORS:` for false checks
- <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/collections_service_pb2.py#L23'>qdrant_client/grpc/collections_service_pb2.py:23:4:</a> E712 Avoid equality comparisons to `False`; use `if not _descriptor._USE_C_DESCRIPTORS:` for false checks
+ <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/collections_service_pb2.py#L23'>qdrant_client/grpc/collections_service_pb2.py:23:4:</a> E712 Avoid equality comparisons to `False`; use `not _descriptor._USE_C_DESCRIPTORS:` for false checks
- <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/json_with_int_pb2.py#L58'>qdrant_client/grpc/json_with_int_pb2.py:58:4:</a> E712 Avoid equality comparisons to `False`; use `if not _descriptor._USE_C_DESCRIPTORS:` for false checks
+ <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/json_with_int_pb2.py#L58'>qdrant_client/grpc/json_with_int_pb2.py:58:4:</a> E712 Avoid equality comparisons to `False`; use `not _descriptor._USE_C_DESCRIPTORS:` for false checks
- <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/points_pb2.py#L1295'>qdrant_client/grpc/points_pb2.py:1295:4:</a> E712 Avoid equality comparisons to `False`; use `if not _descriptor._USE_C_DESCRIPTORS:` for false checks
+ <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/points_pb2.py#L1295'>qdrant_client/grpc/points_pb2.py:1295:4:</a> E712 Avoid equality comparisons to `False`; use `not _descriptor._USE_C_DESCRIPTORS:` for false checks
- <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/points_service_pb2.py#L23'>qdrant_client/grpc/points_service_pb2.py:23:4:</a> E712 Avoid equality comparisons to `False`; use `if not _descriptor._USE_C_DESCRIPTORS:` for false checks
+ <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/points_service_pb2.py#L23'>qdrant_client/grpc/points_service_pb2.py:23:4:</a> E712 Avoid equality comparisons to `False`; use `not _descriptor._USE_C_DESCRIPTORS:` for false checks
- <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/qdrant_pb2.py#L41'>qdrant_client/grpc/qdrant_pb2.py:41:4:</a> E712 Avoid equality comparisons to `False`; use `if not _descriptor._USE_C_DESCRIPTORS:` for false checks
+ <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/qdrant_pb2.py#L41'>qdrant_client/grpc/qdrant_pb2.py:41:4:</a> E712 Avoid equality comparisons to `False`; use `not _descriptor._USE_C_DESCRIPTORS:` for false checks
- <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/snapshots_service_pb2.py#L103'>qdrant_client/grpc/snapshots_service_pb2.py:103:4:</a> E712 Avoid equality comparisons to `False`; use `if not _descriptor._USE_C_DESCRIPTORS:` for false checks
+ <a href='https://github.com/qdrant/qdrant-client/blob/863116e881a547df83d6bb76e89550fa72eb5d86/qdrant_client/grpc/snapshots_service_pb2.py#L103'>qdrant_client/grpc/snapshots_service_pb2.py:103:4:</a> E712 Avoid equality comparisons to `False`; use `not _descriptor._USE_C_DESCRIPTORS:` for false checks
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E712 | 18 | 9 | 9 | 0 | 0 |

</p>
</details>




---

_Label `diagnostics` added by @ntBre on 2025-05-26 23:46_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E712_E712.py.snap`:22 on 2025-05-27 06:04_

The message now is incorrect for `if` statements. It's also incorrect for `assert` because of the trailing `colon`. 

I think the proper suggestion here is to suggest: use `res` for truth checks and use `not res`


---

_@MichaReiser requested changes on 2025-05-27 06:04_

---

_@CodeMan62 reviewed on 2025-05-27 07:14_

---

_Review comment by @CodeMan62 on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E712_E712.py.snap`:22 on 2025-05-27 07:14_

sounds good to me let me update

---

_@CodeMan62 reviewed on 2025-05-27 07:22_

---

_Review comment by @CodeMan62 on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E712_E712.py.snap`:22 on 2025-05-27 07:22_

Done

---

_Comment by @CodeMan62 on 2025-05-27 17:24_

@MichaReiser i have updated the code as you suggest is it good to go now??

---

_@MichaReiser approved on 2025-05-28 07:05_

Thank you

---

_Merged by @MichaReiser on 2025-05-28 07:06_

---

_Closed by @MichaReiser on 2025-05-28 07:06_

---
