```yaml
number: 15083
title: "[`airflow`] Extend rule to check class attributes, methods, arguments (`AIR302`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-AIR302
created_at: 2024-12-20T14:44:55Z
updated_at: 2025-01-08T07:28:02Z
url: https://github.com/astral-sh/ruff/pull/15083
synced_at: 2026-01-10T20:34:00Z
```

# [`airflow`] Extend rule to check class attributes, methods, arguments (`AIR302`)

---

_Pull request opened by @Lee-W on 2024-12-20 14:44_


<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Airflow 3.0 removes various deprecated functions, members, modules, and other values. They have been deprecated in 2.x, but the removal causes incompatibilities that we want to detect. This PR add rules for the following.

* Removed class attribute
    * `airflow.providers_manager.ProvidersManager.dataset_factories` → `airflow.providers_manager.ProvidersManager.asset_factories`
    * `airflow.providers_manager.ProvidersManager.dataset_uri_handlers` → `airflow.providers_manager.ProvidersManager.asset_uri_handlers`
    * `airflow.providers_manager.ProvidersManager.dataset_to_openlineage_converters` → `airflow.providers_manager.ProvidersManager.asset_to_openlineage_converters`
    * `airflow.lineage.hook.DatasetLineageInfo.dataset`  → `airflow.lineage.hook.AssetLineageInfo.asset`
* Removed class method (subclasses in airflow should also checked)
    * `airflow.secrets.base_secrets.BaseSecretsBackend.get_conn_uri` → `airflow.secrets.base_secrets.BaseSecretsBackend.get_conn_value`
    * `airflow.secrets.base_secrets.BaseSecretsBackend.get_connections` → `airflow.secrets.base_secrets.BaseSecretsBackend.get_connection`
    * `airflow.hooks.base.BaseHook.get_connections` → use `get_connection`
    * `airflow.datasets.BaseDataset.iter_datasets` → `airflow.sdk.definitions.asset.BaseAsset.iter_assets`
    * `airflow.datasets.BaseDataset.iter_dataset_aliases` → `airflow.sdk.definitions.asset.BaseAsset.iter_asset_aliases`
* Removed constructor args (subclasses in airflow should also checked)
    * argument `filename_template` in`airflow.utils.log.file_task_handler.FileTaskHandler`
    * in `BaseOperator`
        * `sla`
        * `task_concurrency` → `max_active_tis_per_dag`
    * in `BaseAuthManager`
        * `appbuilder`
* Removed class variable (subclasses anywhere should be checked)
    * in `airflow.plugins_manager.AirflowPlugin`
        * `executors` (from #43289)
        * `hooks`
        * `operators`
        * `sensors`
* Replaced names
	* `airflow.hooks.base_hook.BaseHook` → `airflow.hooks.base.BaseHook`
    * `airflow.operators.dagrun_operator.TriggerDagRunLink` → `airflow.operators.trigger_dagrun.TriggerDagRunLink`
    * `airflow.operators.dagrun_operator.TriggerDagRunOperator` → `airflow.operators.trigger_dagrun.TriggerDagRunOperator`
    * `airflow.operators.python_operator.BranchPythonOperator` → `airflow.operators.python.BranchPythonOperator`
    * `airflow.operators.python_operator.PythonOperator` → `airflow.operators.python.PythonOperator`
    * `airflow.operators.python_operator.PythonVirtualenvOperator` → `airflow.operators.python.PythonVirtualenvOperator`
    * `airflow.operators.python_operator.ShortCircuitOperator` → `airflow.operators.python.ShortCircuitOperator`
    * `airflow.operators.latest_only_operator.LatestOnlyOperator` → `airflow.operators.latest_only.LatestOnlyOperator`


In additional to the changes above, this PR also add utility functions and improve docstring.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
A test fixture is included in the PR.


---

_Renamed from "[airflow]: extend removed method class attributes and more methods(AIR302)" to "[airflow]: extend removed method class attributes and more methods (AIR302)" by @Lee-W on 2024-12-20 14:45_

---

_@MichaReiser reviewed on 2024-12-20 14:50_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:396 on 2024-12-20 14:50_

It should be possible to simplify the regex a bit (to ensure it runs less frequently?)
```suggestion
			["airflow", rest @ ..] if SECRET_BACKEND_REGEX.is_match(rest.join(".")) => match attr.as_str() {
            "get_conn_uri" => Some(Replacement::Name("get_conn_value")),
            "get_connections" => Some(Replacement::Name("get_connection")),
            &_ => None,
        },
```

This requires removing the `airflow` segment from the regex. The advantage is that we can skip over the match if the first segment isn't airflow

---

_Comment by @github-actions[bot] on 2024-12-20 14:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+7 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/52ed7d72e9669a7bdfaf11caeea6daa29911bc8f/providers/tests/alibaba/cloud/log/test_oss_task_handler.py#L189'>providers/tests/alibaba/cloud/log/test_oss_task_handler.py:189:67:</a> AIR302 `filename_template` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/52ed7d72e9669a7bdfaf11caeea6daa29911bc8f/providers/tests/amazon/aws/log/test_cloudwatch_task_handler.py#L244'>providers/tests/amazon/aws/log/test_cloudwatch_task_handler.py:244:13:</a> AIR302 `filename_template` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/52ed7d72e9669a7bdfaf11caeea6daa29911bc8f/providers/tests/amazon/aws/log/test_s3_task_handler.py#L232'>providers/tests/amazon/aws/log/test_s3_task_handler.py:232:70:</a> AIR302 `filename_template` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/52ed7d72e9669a7bdfaf11caeea6daa29911bc8f/providers/tests/elasticsearch/log/test_es_task_handler.py#L673'>providers/tests/elasticsearch/log/test_es_task_handler.py:673:13:</a> AIR302 `filename_template` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/52ed7d72e9669a7bdfaf11caeea6daa29911bc8f/providers/tests/fab/auth_manager/test_fab_auth_manager.py#L87'>providers/tests/fab/auth_manager/test_fab_auth_manager.py:87:26:</a> AIR302 `appbuilder` is removed in Airflow 3.0; The constructor takes no parameter now.
+ <a href='https://github.com/apache/airflow/blob/52ed7d72e9669a7bdfaf11caeea6daa29911bc8f/providers/tests/google/cloud/log/test_gcs_task_handler.py#L295'>providers/tests/google/cloud/log/test_gcs_task_handler.py:295:13:</a> AIR302 `filename_template` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/52ed7d72e9669a7bdfaf11caeea6daa29911bc8f/providers/tests/microsoft/azure/log/test_wasb_task_handler.py#L211'>providers/tests/microsoft/azure/log/test_wasb_task_handler.py:211:13:</a> AIR302 `filename_template` is removed in Airflow 3.0
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR302 | 7 | 7 | 0 | 0 | 0 |

</p>
</details>




---

_@dhruvmanila reviewed on 2024-12-24 06:02_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:16 on 2024-12-24 06:02_

Can you say more about these imports? Is it that certain methods (`get_conn_uri`, `get_connections`) in `Backend` / `Hook` classes are deprecated? Can these classes come from _anywhere_? Or, should it be limited to from either the `airflow.{secrets,hooks}` module or one of the provider module? I'm asking this mainly by referring to the test cases.

If it can only come from either the `airflow.{secrets,hooks}` module or one of the provider module, then we can avoid regex similar to what we've done for `AIR001`:

https://github.com/astral-sh/ruff/blob/f0b03857a606e16974b119b6efad3766a11b0327/crates/ruff_linter/src/rules/airflow/rules/task_variable_name.rs#L69-L94

---

_Label `rule` added by @dhruvmanila on 2024-12-24 06:03_

---

_Label `preview` added by @dhruvmanila on 2024-12-24 06:03_

---

_@Lee-W reviewed on 2024-12-24 23:50_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:16 on 2024-12-24 23:50_

>  Is it that certain methods (get_conn_uri, get_connections) in Backend / Hook classes are deprecated?

Yes

> Can these classes come from anywhere? Or, should it be limited to from either the airflow.{secrets,hooks} module or one of the provider module? 

Technically, it can come from anywhere, but we only want to check the airflow core and airflow providers' usage.

> If it can only come from either the airflow.{secrets,hooks} module or one of the provider module, then we can avoid regex similar to what we've done for AIR001:

Yep, we probably could! Thanks for your review! Let me rewrite it this way

---

_@Lee-W reviewed on 2024-12-25 04:01_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:16 on 2024-12-25 04:01_

Just updated it. Thanks!

---

_@Lee-W reviewed on 2024-12-25 04:01_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:396 on 2024-12-25 04:01_

Just updated it with later comment. Thanks!

---

_Review requested from @dhruvmanila by @Lee-W on 2024-12-25 04:01_

---

_Review requested from @MichaReiser by @Lee-W on 2024-12-25 04:01_

---

_Comment by @Lee-W on 2024-12-29 10:58_

@MichaReiser, Pinging in case we missed this one 🙂 Would be awesome if we could get this re-reviewed when bandwidth allowed. Thanks a lot!

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:401 on 2024-12-30 08:32_

nit: I think we should just combine this in the catch-all like:

```rs
if is_airflow_secret_backend(qualname.segments()) {
	// ...
} else if is_airflow_hook(&qualname) {
	// ...
}
```

Assuming that these are mutually exclusive which I think it is as it's in separate match arms.

---

_@dhruvmanila reviewed on 2024-12-30 08:32_

---

_@dhruvmanila reviewed on 2024-12-30 08:34_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:244 on 2024-12-30 08:34_

nit: same argument as mentioned above to combine the various catch-all into a single `if else` block

---

_@dhruvmanila reviewed on 2024-12-30 08:36_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:14 on 2024-12-30 08:36_

Maybe we could combine all the functions into a single function and accept the changing variables? I think those are the module names and the ends_with value. What about the following?

```rust
fn is_airflow_builtin_or_provider(segments: &[&str], module: &str, symbol_suffix: &str) -> bool {
    match segments {
        ["airflow", "secrets", rest @ ..] => {
            if let Some(last_element) = rest.last() {
                last_element.ends_with(symbol_suffix)
            } else {
                false
            }
        }

        ["airflow", "providers", rest @ ..] => {
            if let (Some(pos), Some(last_element)) =
                (rest.iter().position(|&s| s == module), rest.last())
            {
                pos + 1 < rest.len() && last_element.ends_with(symbol_suffix)
            } else {
                false
            }
        }

        _ => false,
    }
}
```

---

_@Lee-W reviewed on 2024-12-30 14:11_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:401 on 2024-12-30 14:11_

Sure! Updated!

---

_@Lee-W reviewed on 2024-12-30 14:11_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:244 on 2024-12-30 14:11_

Sure! Updated!

---

_@Lee-W reviewed on 2024-12-30 14:12_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:14 on 2024-12-30 14:12_

I tried to refactor the utility functions a bit. Please take a look when you're available. Thanks!

---

_Review requested from @dhruvmanila by @Lee-W on 2024-12-30 14:25_

---

_@dhruvmanila reviewed on 2024-12-30 15:22_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:28 on 2024-12-30 15:22_

Should the `starts_with` be instead a `start_element == module` as I think we need to match the module exactly?

Also, I think this should be `&&` and not `&` (bitwise AND) ;)

---

_@dhruvmanila reviewed on 2024-12-30 15:23_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:28 on 2024-12-30 15:23_

The `&&` typo makes me wonder how come the tests failed to catch this

---

_@dhruvmanila reviewed on 2024-12-30 15:53_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:28 on 2024-12-30 15:53_

I'm unable to push to this PR, so here's the change that I made locally to fix this and add some documentation:

```diff
diff --git a/crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs b/crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs
index 0f6831317..592329fde 100644
--- a/crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs
+++ b/crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs
@@ -11,21 +11,35 @@ use ruff_text_size::Ranged;
 
 use crate::checkers::ast::Checker;
 
+/// Check whether the segments corresponding to the fully qualified name points to a symbol that's
+/// either a builtin or coming from one of the providers in Airflow.
+///
+/// The pattern it looks for are:
+/// - `airflow.providers.**.<module>.**.*<symbol_suffix>` for providers
+/// - `airflow.<module>.**.*<symbol_suffix>` for builtins
+///
+/// where `**` is one or more segments separated by a dot, and `*` is one or more characters.
+///
+/// Examples for the above patterns:
+/// - `airflow.providers.google.cloud.secrets.secret_manager.CloudSecretManagerBackend` (provider)
+/// - `airflow.secrets.base_secrets.BaseSecretsBackend` (builtin)
 fn is_airflow_builtin_or_provider(segments: &[&str], module: &str, symbol_suffix: &str) -> bool {
     match segments {
         ["airflow", "providers", rest @ ..] => {
             if let (Some(pos), Some(last_element)) =
                 (rest.iter().position(|&s| s == module), rest.last())
             {
+                // Check that the module is not the last element i.e., there's a symbol that's
+                // being used from the `module` that ends with `symbol_suffix`.
                 pos + 1 < rest.len() && last_element.ends_with(symbol_suffix)
             } else {
                 false
             }
         }
 
-        ["airflow", rest @ ..] => {
-            if let (Some(start_element), Some(last_element)) = (rest.first(), rest.last()) {
-                start_element.starts_with(module) & last_element.ends_with(symbol_suffix)
+        ["airflow", first, rest @ ..] => {
+            if let Some(last) = rest.last() {
+                first == module && last.ends_with(symbol_suffix)
             } else {
                 false
             }
@@ -35,18 +49,26 @@ fn is_airflow_builtin_or_provider(segments: &[&str], module: &str, symbol_suffix
     }
 }
 
+/// Check whether the symbol is coming from the `secrets` builtin or provider module which ends
+/// with `Backend`.
 fn is_airflow_secret_backend(segments: &[&str]) -> bool {
     is_airflow_builtin_or_provider(segments, "secrets", "Backend")
 }
 
+/// Check whether the symbol is coming from the `hooks` builtin or provider module which ends
+/// with `Hook`.
 fn is_airflow_hook(segments: &[&str]) -> bool {
     is_airflow_builtin_or_provider(segments, "hooks", "Hook")
 }
 
+/// Check whether the symbol is coming from the `operators` builtin or provider module which ends
+/// with `Operator`.
 fn is_airflow_operator(segments: &[&str]) -> bool {
     is_airflow_builtin_or_provider(segments, "operators", "Operator")
 }
 
+/// Check whether the symbol is coming from the `log` builtin or provider module which ends
+/// with `TaskHandler`.
 fn is_airflow_task_handler(segments: &[&str]) -> bool {
     is_airflow_builtin_or_provider(segments, "log", "TaskHandler")
 }
```

---

_@dhruvmanila reviewed on 2024-12-30 15:54_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:28 on 2024-12-30 15:54_

Additionally, as convention these helper functions would go at the bottom after the rule logic i.e., the rule definition would be up top and then the entrypoint to the rule logic and then all the helper structs and functions.

---

_@Lee-W reviewed on 2024-12-31 00:53_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:28 on 2024-12-31 00:53_

@dhruvmanila  Thanks! This helps a ton! Just updated it and add more docstring to each utility functions

---

_Review requested from @dhruvmanila by @Lee-W on 2024-12-31 00:53_

---

_Renamed from "[airflow]: extend removed method class attributes and more methods (AIR302)" to "[`airflow`] Extend rule to check class attributes, methods, arguments (`AIR302`)" by @dhruvmanila on 2024-12-31 04:17_

---

_@dhruvmanila reviewed on 2024-12-31 04:18_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:112 on 2024-12-31 04:18_

In Rust, documentation comments starts with triple forward slashes (`///`). I'll update this in my planned follow-up PR.

---

_@dhruvmanila approved on 2024-12-31 04:19_

Thank you!

I've a few minor nits but I can't push it to this branch, I'll create a follow-up PR instead.

---

_Merged by @dhruvmanila on 2024-12-31 04:19_

---

_Closed by @dhruvmanila on 2024-12-31 04:19_

---

_@Lee-W reviewed on 2024-12-31 07:34_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:112 on 2024-12-31 07:34_

Got it!

---

_Comment by @Lee-W on 2024-12-31 07:35_

> Thank you!
> 
> I've a few minor nits but I can't push it to this branch, I'll create a follow-up PR instead.

Thanks a ton! Not sure why you cannot push to this branch 😢 I read through the refactor PR. Everything looks much better! But I noticed there one minor typo and created a PR for this fix https://github.com/astral-sh/ruff/pull/15211

---

_Branch deleted on 2025-01-08 07:28_

---
