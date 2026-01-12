```yaml
number: 22057
title: "[flake8-blind-except] Allowed exception logging with functions other than ``critical``, ``error`` and ``exception`` (BLE001)"
type: pull_request
state: open
author: lubaskinc0de
labels:
  - rule
  - preview
assignees: []
base: main
head: feat/21889-ble001
created_at: 2025-12-18T18:40:38Z
updated_at: 2026-01-09T22:25:31Z
url: https://github.com/astral-sh/ruff/pull/22057
synced_at: 2026-01-12T15:57:40Z
```

# [flake8-blind-except] Allowed exception logging with functions other than ``critical``, ``error`` and ``exception`` (BLE001)

---

_@lubaskinc0de_

## Summary
Fix issue #21889 by checking that the logging method is one of the ``debug``, ``info``, ``warning``, ``error``, ``exception``, ``critical``, ``log`` methods that support ``exc_info`` passing. Also fixed the behavior in which ``exc_info`` was considered passed only when it was equal to the literal ``True``, now the ``Truthiness`` of the expression is checked (we will leave additional checks to type checkers)

## Test Plan
Additional snapshot tests have been added for all logging functions, as well as tests in which an exception object is passed as ``exc_info``


---

_Comment by @astral-sh-bot[bot] on 2025-12-18 18:51_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -32 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -26 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/airflow-core/src/airflow/api_fastapi/execution_api/app.py#L152'>airflow-core/src/airflow/api_fastapi/execution_api/app.py:152:20:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/airflow-core/src/airflow/api_fastapi/execution_api/deps.py#L91'>airflow-core/src/airflow/api_fastapi/execution_api/deps.py:91:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/airflow-core/src/airflow/jobs/scheduler_job_runner.py#L2357'>airflow-core/src/airflow/jobs/scheduler_job_runner.py:2357:20:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/airflow-core/src/airflow/models/dagrun.py#L1665'>airflow-core/src/airflow/models/dagrun.py:1665:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/airflow-core/src/airflow/utils/serve_logs/log_server.py#L111'>airflow-core/src/airflow/utils/serve_logs/log_server.py:111:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/providers/amazon/src/airflow/providers/amazon/aws/utils/suppress.py#L65'>providers/amazon/src/airflow/providers/amazon/aws/utils/suppress.py:65:20:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/providers/ftp/src/airflow/providers/ftp/operators/ftp.py#L158'>providers/ftp/src/airflow/providers/ftp/operators/ftp.py:158:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/providers/google/src/airflow/providers/google/cloud/openlineage/mixins.py#L123'>providers/google/src/airflow/providers/google/cloud/openlineage/mixins.py:123:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/providers/google/src/airflow/providers/google/cloud/operators/dataproc.py#L1802'>providers/google/src/airflow/providers/google/cloud/operators/dataproc.py:1802:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/providers/google/src/airflow/providers/google/cloud/operators/dataproc.py#L1999'>providers/google/src/airflow/providers/google/cloud/operators/dataproc.py:1999:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/providers/google/src/airflow/providers/google/cloud/operators/dataproc.py#L2589'>providers/google/src/airflow/providers/google/cloud/operators/dataproc.py:2589:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/providers/google/src/airflow/providers/google/cloud/transfers/bigquery_to_sql.py#L174'>providers/google/src/airflow/providers/google/cloud/transfers/bigquery_to_sql.py:174:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/providers/openlineage/src/airflow/providers/openlineage/extractors/base.py#L158'>providers/openlineage/src/airflow/providers/openlineage/extractors/base.py:158:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/providers/openlineage/src/airflow/providers/openlineage/extractors/manager.py#L140'>providers/openlineage/src/airflow/providers/openlineage/extractors/manager.py:140:20:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/providers/openlineage/src/airflow/providers/openlineage/plugins/adapter.py#L169'>providers/openlineage/src/airflow/providers/openlineage/plugins/adapter.py:169:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/providers/openlineage/src/airflow/providers/openlineage/plugins/listener.py#L691'>providers/openlineage/src/airflow/providers/openlineage/plugins/listener.py:691:16:</a> BLE001 Do not catch blind exception: `BaseException`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/providers/openlineage/src/airflow/providers/openlineage/plugins/listener.py#L743'>providers/openlineage/src/airflow/providers/openlineage/plugins/listener.py:743:16:</a> BLE001 Do not catch blind exception: `BaseException`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/providers/openlineage/src/airflow/providers/openlineage/plugins/listener.py#L796'>providers/openlineage/src/airflow/providers/openlineage/plugins/listener.py:796:16:</a> BLE001 Do not catch blind exception: `BaseException`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/providers/openlineage/src/airflow/providers/openlineage/sqlparser.py#L355'>providers/openlineage/src/airflow/providers/openlineage/sqlparser.py:355:20:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L1352'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:1352:12:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L1560'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:1560:20:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/providers/sftp/src/airflow/providers/sftp/operators/sftp.py#L236'>providers/sftp/src/airflow/providers/sftp/operators/sftp.py:236:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/providers/ssh/src/airflow/providers/ssh/hooks/ssh.py#L508'>providers/ssh/src/airflow/providers/ssh/hooks/ssh.py:508:24:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/providers/standard/src/airflow/providers/standard/utils/openlineage.py#L178'>providers/standard/src/airflow/providers/standard/utils/openlineage.py:178:12:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/task-sdk/src/airflow/sdk/execution_time/task_runner.py#L963'>task-sdk/src/airflow/sdk/execution_time/task_runner.py:963:12:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/ffd66c00d07d148e915755a0ae818c4be1750799/task-sdk/src/airflow/sdk/serde/serializers/kubernetes.py#L59'>task-sdk/src/airflow/sdk/serde/serializers/kubernetes.py:59:16:</a> BLE001 Do not catch blind exception: `Exception`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/53dddf4db26d32ec86673a6d42b009cc49fbd09d/superset/db_engine_specs/bigquery.py#L868'>superset/db_engine_specs/bigquery.py:868:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/superset/blob/53dddf4db26d32ec86673a6d42b009cc49fbd09d/superset/db_engine_specs/bigquery.py#L915'>superset/db_engine_specs/bigquery.py:915:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/superset/blob/53dddf4db26d32ec86673a6d42b009cc49fbd09d/superset/models/core.py#L1199'>superset/models/core.py:1199:20:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/superset/blob/53dddf4db26d32ec86673a6d42b009cc49fbd09d/superset/security/manager.py#L2888'>superset/security/manager.py:2888:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/superset/blob/53dddf4db26d32ec86673a6d42b009cc49fbd09d/superset/utils/screenshots.py#L294'>superset/utils/screenshots.py:294:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/superset/blob/53dddf4db26d32ec86673a6d42b009cc49fbd09d/superset/utils/screenshots.py#L300'>superset/utils/screenshots.py:300:20:</a> BLE001 Do not catch blind exception: `Exception`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/18c25e9f10492c6b78a0bce1d65e6a29554bc5c0/libs/langchain/langchain_classic/chains/openai_functions/openapi.py#L383'>libs/langchain/langchain_classic/chains/openai_functions/openapi.py:383:32:</a> RUF100 [*] Unused `noqa` directive (unused: `BLE001`)
+ <a href='https://github.com/langchain-ai/langchain/blob/18c25e9f10492c6b78a0bce1d65e6a29554bc5c0/libs/langchain/langchain_classic/smith/evaluation/runner_utils.py#L1208'>libs/langchain/langchain_classic/smith/evaluation/runner_utils.py:1208:37:</a> RUF100 [*] Unused `noqa` directive (unused: `BLE001`)
+ <a href='https://github.com/langchain-ai/langchain/blob/18c25e9f10492c6b78a0bce1d65e6a29554bc5c0/libs/langchain/langchain_classic/smith/evaluation/runner_utils.py#L1216'>libs/langchain/langchain_classic/smith/evaluation/runner_utils.py:1216:33:</a> RUF100 [*] Unused `noqa` directive (unused: `BLE001`)
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| BLE001 | 32 | 0 | 32 | 0 | 0 |
| RUF100 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>





---

_Renamed from "[flake8-blind-except] Allowed exception logging with functions other than critical and error (BLE001)" to "[flake8-blind-except] Allowed exception logging with functions other than ``critical``, ``error`` and ``exception`` (BLE001)" by @lubaskinc0de on 2025-12-18 18:54_

---

_@chirizxc reviewed on 2025-12-19 11:42_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_blind_except/rules/blind_except.rs`:133 on 2025-12-19 11:42_

Can we take this function and move it to a `helpers.rs` (and make `pub(crate)`? So as not to duplicate it?

https://github.com/astral-sh/ruff/blob/main/crates%2Fruff_linter%2Fsrc%2Frules%2Fflake8_logging%2Frules%2Froot_logger_call.rs#L70-L76

---

_@lubaskinc0de reviewed on 2025-12-22 12:32_

---

_Review comment by @lubaskinc0de on `crates/ruff_linter/src/rules/flake8_blind_except/rules/blind_except.rs`:133 on 2025-12-22 12:32_

Fixed

---

_@chirizxc reviewed on 2025-12-22 13:02_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_blind_except/rules/blind_except.rs`:11 on 2025-12-22 13:02_

```diff
- use crate::helpers::is_logger_method_name;
use ruff_macros::{ViolationMetadata, derive_message_formats};
use ruff_python_ast::helpers::Truthiness;
use ruff_python_ast::statement_visitor::{StatementVisitor, walk_stmt};
use ruff_python_ast::{self as ast, Expr, Stmt};
use ruff_python_semantic::SemanticModel;
use ruff_python_semantic::analyze::logging;
use ruff_text_size::Ranged;

use crate::Violation;
use crate::checkers::ast::Checker;
+ use crate::helpers::is_logger_method_name;
````

---

_@chirizxc reviewed on 2025-12-22 13:03_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_logging/rules/root_logger_call.rs`:7 on 2025-12-22 13:03_

```diff
- use crate::helpers::is_logger_method_name;
use ruff_macros::{ViolationMetadata, derive_message_formats};
use ruff_python_ast::ExprCall;
use ruff_python_semantic::Modules;

use crate::Violation;
use crate::checkers::ast::Checker;
+ use crate::helpers::is_logger_method_name;
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_blind_except/rules/blind_except.rs`:232 on 2025-12-24 20:22_

I think we should factor this out into a helper function since we use it twice and it's a bit unwieldy as a closure anyway.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_blind_except/BLE.py`:1 on 2025-12-24 20:26_

I always feel bad complaining about too many tests, but this is _a lot_ of tests for a relatively straightforward change. I think it does make sense to have one case for each `logging` method, but I don't think we need to test the whole matrix of `logging` method, `exc_info` truthiness, and importing the names from the `logging` module. The latter two parts are shared regardless of the method name.

In short, I think I'd strip this down to one case like:

```py
try:
	pass
except Exception as e:
	logging.error("...", exc_info=True)  # or `e`, just something truthy
```

for each newly supported logging method. We can also leave out any that were already tested, such as `critical`, `error`, and `exception`.

We should also have one case for the new `Truthiness` check, but one should be enough.

---

_@ntBre reviewed on 2025-12-24 20:33_

Thank you! This looks great overall. I Just had one suggestion about trimming down the number of test cases and one to share some code for the `Truthiness` checks. 

I also think we should make this a [`preview`](https://github.com/astral-sh/ruff/blob/c0fb235a703684bca1cc1767015bb7e19ab1c22f/crates/ruff_linter/src/preview.rs#L16-L19) (link is to an example preview function) change since the ecosystem check is showing a number of changes, including some now-unused `noqa` comments.

---

_Label `rule` added by @ntBre on 2025-12-24 20:33_

---

_Label `preview` added by @ntBre on 2025-12-24 20:33_

---

_Review requested from @ntBre by @lubaskinc0de on 2025-12-27 21:55_

---

_Review requested from @chirizxc by @lubaskinc0de on 2025-12-30 17:33_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_blind_except/mod.rs`:41 on 2026-01-01 14:41_

We have a relatively new `assert_diagnostics_diff!` macro that might be nice here. It can show a diff of the stable and preview diagnostics, so it's a bit nicer than having to compare them manually:

https://github.com/astral-sh/ruff/blob/b2b9d91859f2abc017e35d22f01696f141f6f5b1/crates/ruff_linter/src/rules/flake8_builtins/mod.rs#L74

The linked example is diffing Python versions, but it should be pretty similar for preview.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_blind_except/rules/blind_except.rs`:128 on 2026-01-01 14:45_

This looks a bit redundant right now, but I'm leaning toward leaving it like this because it should make it slightly easier to remove the preview gating later. Otherwise I would suggest removing the separate `logger_objects` argument and just extracting that from the full settings in `new`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_blind_except/rules/blind_except.rs`:202 on 2026-01-01 14:52_

Small nit:

```suggestion
                // Treat `Truthiness::Unknown` as true
                .unwrap_or(true)
```

We could also use this instead, which I saw in some other `into_bool` references. It might convey the meaning slightly better and we could omit the comment:


```suggestion
                != Some(false)
```

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_blind_except/BLE.py`:85 on 2026-01-01 14:55_

Sorry for another test nit, but could you move the new cases to the bottom of the file? That avoids the churn in the snapshot line numbers. And annotating the logging lines with either `# ok` or `# BLE001` depending on if we expect a diagnostic would also be a big help (although I think these are all okay?)

---

_@ntBre reviewed on 2026-01-01 14:57_

Thanks! A few more small comments.

---

_@lubaskinc0de reviewed on 2026-01-01 16:08_

---

_Review comment by @lubaskinc0de on `crates/ruff_linter/src/rules/flake8_blind_except/rules/blind_except.rs`:128 on 2026-01-01 16:08_

Yes, I did this with the expectation that the preview would be easy to remove in the future, so I think it's best to leave it that way

---

_Review requested from @ntBre by @lubaskinc0de on 2026-01-03 10:55_

---
