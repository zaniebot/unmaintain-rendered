```yaml
number: 15449
title: "[`flake8-pytest-style`] Test function parameters with default arguments (`PT028`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: PT028
created_at: 2025-01-13T02:14:34Z
updated_at: 2025-01-13T14:08:21Z
url: https://github.com/astral-sh/ruff/pull/15449
synced_at: 2026-01-12T15:55:51Z
```

# [`flake8-pytest-style`] Test function parameters with default arguments (`PT028`)

---

_@InSyncWithFoo_

## Summary

Resolves #15412.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-13 02:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+95 -0 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/fcb706cca4ff224daf70a92a6ea11fc7cb1736bd/tests/formulary/test_thermal_speed.py#L400'>tests/formulary/test_thermal_speed.py:400:43:</a> PT028 Test function parameter `kwargs` has default argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+49 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/helm_tests/airflow_aux/test_container_lifecycle.py#L102'>helm_tests/airflow_aux/test_container_lifecycle.py:102:54:</a> PT028 Test function parameter `hook_type` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/helm_tests/airflow_aux/test_container_lifecycle.py#L123'>helm_tests/airflow_aux/test_container_lifecycle.py:123:59:</a> PT028 Test function parameter `hook_type` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/helm_tests/airflow_aux/test_container_lifecycle.py#L168'>helm_tests/airflow_aux/test_container_lifecycle.py:168:65:</a> PT028 Test function parameter `hook_type` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/helm_tests/airflow_aux/test_container_lifecycle.py#L187'>helm_tests/airflow_aux/test_container_lifecycle.py:187:64:</a> PT028 Test function parameter `hook_type` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/helm_tests/airflow_aux/test_container_lifecycle.py#L208'>helm_tests/airflow_aux/test_container_lifecycle.py:208:68:</a> PT028 Test function parameter `hook_type` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/helm_tests/airflow_aux/test_container_lifecycle.py#L41'>helm_tests/airflow_aux/test_container_lifecycle.py:41:52:</a> PT028 Test function parameter `hook_type` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/helm_tests/airflow_aux/test_container_lifecycle.py#L78'>helm_tests/airflow_aux/test_container_lifecycle.py:78:45:</a> PT028 Test function parameter `hook_type` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/helm_tests/airflow_aux/test_pod_template_file.py#L927'>helm_tests/airflow_aux/test_pod_template_file.py:927:84:</a> PT028 Test function parameter `hook_type` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/amazon/aws/hooks/test_eks.py#L1029'>providers/tests/amazon/aws/hooks/test_eks.py:1029:66:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/amazon/aws/hooks/test_eks.py#L243'>providers/tests/amazon/aws/hooks/test_eks.py:243:58:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/amazon/aws/hooks/test_eks.py#L253'>providers/tests/amazon/aws/hooks/test_eks.py:253:58:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/amazon/aws/hooks/test_eks.py#L263'>providers/tests/amazon/aws/hooks/test_eks.py:263:58:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/amazon/aws/hooks/test_eks.py#L332'>providers/tests/amazon/aws/hooks/test_eks.py:332:58:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/amazon/aws/hooks/test_eks.py#L350'>providers/tests/amazon/aws/hooks/test_eks.py:350:58:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/amazon/aws/hooks/test_eks.py#L362'>providers/tests/amazon/aws/hooks/test_eks.py:362:58:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/amazon/aws/hooks/test_eks.py#L373'>providers/tests/amazon/aws/hooks/test_eks.py:373:58:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/amazon/aws/hooks/test_eks.py#L402'>providers/tests/amazon/aws/hooks/test_eks.py:402:60:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/amazon/aws/hooks/test_eks.py#L412'>providers/tests/amazon/aws/hooks/test_eks.py:412:60:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/amazon/aws/hooks/test_eks.py#L445'>providers/tests/amazon/aws/hooks/test_eks.py:445:60:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/amazon/aws/hooks/test_eks.py#L473'>providers/tests/amazon/aws/hooks/test_eks.py:473:60:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/amazon/aws/hooks/test_eks.py#L640'>providers/tests/amazon/aws/hooks/test_eks.py:640:60:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/amazon/aws/hooks/test_eks.py#L654'>providers/tests/amazon/aws/hooks/test_eks.py:654:60:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/amazon/aws/hooks/test_eks.py#L686'>providers/tests/amazon/aws/hooks/test_eks.py:686:60:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/amazon/aws/hooks/test_eks.py#L809'>providers/tests/amazon/aws/hooks/test_eks.py:809:66:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/amazon/aws/hooks/test_eks.py#L819'>providers/tests/amazon/aws/hooks/test_eks.py:819:66:</a> PT028 Test function parameter `initial_batch_size` has default argument
... 24 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/aae8fda11dafdd96298884962e790d10bd0104d0/superset/cli/test_db.py#L141'>superset/cli/test_db.py:141:66:</a> PT028 Test function parameter `raw_engine_kwargs` has default argument
+ <a href='https://github.com/apache/superset/blob/aae8fda11dafdd96298884962e790d10bd0104d0/tests/integration_tests/db_engine_specs/base_engine_spec_tests.py#L50'>tests/integration_tests/db_engine_specs/base_engine_spec_tests.py:50:63:</a> PT028 Test function parameter `engine_spec_class` has default argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/7415aca37159a99f8f99d93a1908070ddf36178c/asv_bench/benchmarks/gil.py#L39'>asv_bench/benchmarks/gil.py:39:31:</a> PT028 Test function parameter `num_threads` has default argument
+ <a href='https://github.com/pandas-dev/pandas/blob/7415aca37159a99f8f99d93a1908070ddf36178c/asv_bench/benchmarks/gil.py#L39'>asv_bench/benchmarks/gil.py:39:46:</a> PT028 Test function parameter `kwargs_list` has default argument
+ <a href='https://github.com/pandas-dev/pandas/blob/7415aca37159a99f8f99d93a1908070ddf36178c/pandas/util/_tester.py#L15'>pandas/util/_tester.py:15:41:</a> PT028 Test function parameter `extra_args` has default argument
+ <a href='https://github.com/pandas-dev/pandas/blob/7415aca37159a99f8f99d93a1908070ddf36178c/pandas/util/_tester.py#L15'>pandas/util/_tester.py:15:68:</a> PT028 Test function parameter `run_doctests` has default argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/658e34248035b7e914ea25c90b4344a4ad72f104/tools/lib/test_server.py#L56'>tools/lib/test_server.py:56:34:</a> PT028 Test function parameter `skip_provision_check` has default argument
+ <a href='https://github.com/zulip/zulip/blob/658e34248035b7e914ea25c90b4344a4ad72f104/tools/lib/test_server.py#L57'>tools/lib/test_server.py:57:26:</a> PT028 Test function parameter `external_host` has default argument
+ <a href='https://github.com/zulip/zulip/blob/658e34248035b7e914ea25c90b4344a4ad72f104/tools/lib/test_server.py#L58'>tools/lib/test_server.py:58:28:</a> PT028 Test function parameter `log_file` has default argument
+ <a href='https://github.com/zulip/zulip/blob/658e34248035b7e914ea25c90b4344a4ad72f104/tools/lib/test_server.py#L59'>tools/lib/test_server.py:59:18:</a> PT028 Test function parameter `dots` has default argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+35 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/81e9ac2a6e661ee2a67fe49612430ef0f933e9dd/astropy/io/votable/tests/test_vo.py#L950'>astropy/io/votable/tests/test_vo.py:950:36:</a> PT028 Test function parameter `test_path_object` has default argument
+ <a href='https://github.com/astropy/astropy/blob/81e9ac2a6e661ee2a67fe49612430ef0f933e9dd/astropy/modeling/tests/test_models.py#L47'>astropy/modeling/tests/test_models.py:47:41:</a> PT028 Test function parameter `amplitude` has default argument
+ <a href='https://github.com/astropy/astropy/blob/81e9ac2a6e661ee2a67fe49612430ef0f933e9dd/astropy/modeling/tests/test_models.py#L47'>astropy/modeling/tests/test_models.py:47:54:</a> PT028 Test function parameter `frequency` has default argument
+ <a href='https://github.com/astropy/astropy/blob/81e9ac2a6e661ee2a67fe49612430ef0f933e9dd/astropy/stats/tests/test_bayesian_blocks.py#L10'>astropy/stats/tests/test_bayesian_blocks.py:10:36:</a> PT028 Test function parameter `rseed` has default argument
+ <a href='https://github.com/astropy/astropy/blob/81e9ac2a6e661ee2a67fe49612430ef0f933e9dd/astropy/stats/tests/test_bayesian_blocks.py#L167'>astropy/stats/tests/test_bayesian_blocks.py:167:35:</a> PT028 Test function parameter `rseed` has default argument
+ <a href='https://github.com/astropy/astropy/blob/81e9ac2a6e661ee2a67fe49612430ef0f933e9dd/astropy/stats/tests/test_bayesian_blocks.py#L20'>astropy/stats/tests/test_bayesian_blocks.py:20:33:</a> PT028 Test function parameter `rseed` has default argument
+ <a href='https://github.com/astropy/astropy/blob/81e9ac2a6e661ee2a67fe49612430ef0f933e9dd/astropy/stats/tests/test_bayesian_blocks.py#L35'>astropy/stats/tests/test_bayesian_blocks.py:35:47:</a> PT028 Test function parameter `rseed` has default argument
+ <a href='https://github.com/astropy/astropy/blob/81e9ac2a6e661ee2a67fe49612430ef0f933e9dd/astropy/stats/tests/test_histogram.py#L101'>astropy/stats/tests/test_histogram.py:101:38:</a> PT028 Test function parameter `N` has default argument
+ <a href='https://github.com/astropy/astropy/blob/81e9ac2a6e661ee2a67fe49612430ef0f933e9dd/astropy/stats/tests/test_histogram.py#L101'>astropy/stats/tests/test_histogram.py:101:50:</a> PT028 Test function parameter `rseed` has default argument
+ <a href='https://github.com/astropy/astropy/blob/81e9ac2a6e661ee2a67fe49612430ef0f933e9dd/astropy/stats/tests/test_histogram.py#L112'>astropy/stats/tests/test_histogram.py:112:43:</a> PT028 Test function parameter `N` has default argument
+ <a href='https://github.com/astropy/astropy/blob/81e9ac2a6e661ee2a67fe49612430ef0f933e9dd/astropy/stats/tests/test_histogram.py#L112'>astropy/stats/tests/test_histogram.py:112:55:</a> PT028 Test function parameter `rseed` has default argument
+ <a href='https://github.com/astropy/astropy/blob/81e9ac2a6e661ee2a67fe49612430ef0f933e9dd/astropy/stats/tests/test_histogram.py#L172'>astropy/stats/tests/test_histogram.py:172:30:</a> PT028 Test function parameter `N` has default argument
+ <a href='https://github.com/astropy/astropy/blob/81e9ac2a6e661ee2a67fe49612430ef0f933e9dd/astropy/stats/tests/test_histogram.py#L172'>astropy/stats/tests/test_histogram.py:172:42:</a> PT028 Test function parameter `rseed` has default argument
+ <a href='https://github.com/astropy/astropy/blob/81e9ac2a6e661ee2a67fe49612430ef0f933e9dd/astropy/stats/tests/test_histogram.py#L17'>astropy/stats/tests/test_histogram.py:17:28:</a> PT028 Test function parameter `N` has default argument
+ <a href='https://github.com/astropy/astropy/blob/81e9ac2a6e661ee2a67fe49612430ef0f933e9dd/astropy/stats/tests/test_histogram.py#L17'>astropy/stats/tests/test_histogram.py:17:41:</a> PT028 Test function parameter `rseed` has default argument
+ <a href='https://github.com/astropy/astropy/blob/81e9ac2a6e661ee2a67fe49612430ef0f933e9dd/astropy/stats/tests/test_histogram.py#L31'>astropy/stats/tests/test_histogram.py:31:31:</a> PT028 Test function parameter `N` has default argument
+ <a href='https://github.com/astropy/astropy/blob/81e9ac2a6e661ee2a67fe49612430ef0f933e9dd/astropy/stats/tests/test_histogram.py#L31'>astropy/stats/tests/test_histogram.py:31:44:</a> PT028 Test function parameter `rseed` has default argument
+ <a href='https://github.com/astropy/astropy/blob/81e9ac2a6e661ee2a67fe49612430ef0f933e9dd/astropy/stats/tests/test_histogram.py#L60'>astropy/stats/tests/test_histogram.py:60:28:</a> PT028 Test function parameter `N` has default argument
... 17 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT028 | 95 | 95 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @InSyncWithFoo on 2025-01-13 02:48_

`is_likely_pytest_test()` and `is_likely_pytest_test_file()` were taken almost verbatim from #15158. The only change is that the former now no longer calls the latter. This is because `flake8-pytest-style` says a function is a test function [when its name starts with `test_`](https://github.com/m-burst/flake8-pytest-style/blob/2c53993e5c5cfc1f4d5e16f30a8e700532cd6f5b/flake8_pytest_style/utils.py#L275-L277).

Aside from that change, I think `is_likely_pytest_test()`'s divergence is for the better. I can't say the same about `is_likely_pytest_test_file()`, however, so it is currently dead code. What should be done with these two?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/helpers.rs`:66 on 2025-01-13 08:07_

```suggestion
    if !func.name.starts_with("test_") {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/helpers.rs`:64 on 2025-01-13 08:09_

We should delete this function if it isn't used anywhere other than in a docstring.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/test_functions.rs`:78 on 2025-01-13 08:13_

I'm not sure if we should provide a fix here because it is likely to break the test:

* I'd have to test this but isn't the pytest call going to fail without the default value? 
* Assuming the call succeeds, `a` now no longer has the value `1` and instead some other default value (`None`?) This can lead to all sort of failures when running the test.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/test_functions.rs`:67 on 2025-01-13 08:15_

This differs from the upstream rule that intentionally ignores `posonly` arguments

https://github.com/m-burst/flake8-pytest-style/blob/92c080312e36c13d2d7725d7bbd89d75d7eaae65/flake8_pytest_style/visitors/t_functions.py#L16-L23

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/snapshots/ruff_linter__rules__flake8_pytest_style__tests__PT028.snap`:4 on 2025-01-13 08:16_

Let's add some tests similar to the ones in the upstream implementation that verify the correct behavior for different parameter kinds.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/helpers.rs`:63 on 2025-01-13 08:20_

Are there cases where this method returns `false` for a proper `pytest`? If not, I suggest renaming it ti `is_pytest_test`. 

We should document whether the function has false negatives or false positives and, if so, when they occur.

Let's also include this a link to https://docs.pytest.org/en/stable/explanation/goodpractices.html#conventions-for-python-test-discovery . I found it a very useful reference to have 

---

_@MichaReiser requested changes on 2025-01-13 08:21_

Thank you. I've a few comments regarding the implementation and I think we should remove the fix. 

---

_Label `rule` added by @MichaReiser on 2025-01-13 08:21_

---

_Label `preview` added by @MichaReiser on 2025-01-13 08:21_

---

_Comment by @MichaReiser on 2025-01-13 08:23_

Did you review the ecosystem changes? 

---

_@InSyncWithFoo reviewed on 2025-01-13 08:49_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/test_functions.rs`:67 on 2025-01-13 08:49_

That block is for `PT019`.

---

_@InSyncWithFoo reviewed on 2025-01-13 08:52_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/test_functions.rs`:78 on 2025-01-13 08:52_

I think the fix goes well with the help message ("Remove default argument"); should it be marked display-only instead?

---

_@InSyncWithFoo reviewed on 2025-01-13 08:59_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/helpers.rs`:66 on 2025-01-13 08:59_

This would be incorrect. [The conventions guide](https://docs.pytest.org/en/stable/explanation/goodpractices.html#conventions-for-python-test-discovery) says test functions start with `test` (i.e. including `testFoo`), not `test_`.

---

_Comment by @InSyncWithFoo on 2025-01-13 09:18_

@MichaReiser Yes. There are a couple of false positives. For example:

[@pandas-dev/pandas](https://github.com/pandas-dev/pandas/blob/57d248978a4168aa73871a62ab79c47dc2977bb0/asv_bench/benchmarks/gil.py#L39):

```python
def test_parallel(num_threads=2, kwargs_list=None):
    """
    Decorator to run the same function multiple times in parallel.
    ...
    """
    ...
```

[@zulip/zulip](https://github.com/zulip/zulip/blob/658e34248035b7e914ea25c90b4344a4ad72f104/tools/lib/test_server.py#L55):

```python
def test_server_running(
    skip_provision_check: bool = False,
    external_host: str = "testserver",
    log_file: str | None = None,
    dots: bool = False,
) -> Iterator[None]:
    ...
```

These kinds of false positives can be reduced (the former would no longer be reported) by making `is_likely_pytest_test()` call `is_likely_pytest_test_file()`. That would be quite a divergence from upstream, however, as discussed above.

---

_@InSyncWithFoo reviewed on 2025-01-13 09:21_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/helpers.rs`:63 on 2025-01-13 09:21_

This function can never be perfectly correct due to user configurations (namely, `testpaths` and `norecursedirs`). Therefore, I think `is_pytest_test` is misleading as a name.

---

_@MichaReiser reviewed on 2025-01-13 09:28_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/helpers.rs`:63 on 2025-01-13 09:28_

I understand that those configuration options only applying to `is_pytest_file`. So for long as we aren't checking `is_pytest_file` in `is_pytest_test`, the function should be fine?

---

_@MichaReiser reviewed on 2025-01-13 09:29_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/helpers.rs`:66 on 2025-01-13 09:29_

The upstream implementation uses `test_`: https://github.com/m-burst/flake8-pytest-style/blob/92c080312e36c13d2d7725d7bbd89d75d7eaae65/flake8_pytest_style/visitors/t_functions.py#L19

---

_@MichaReiser reviewed on 2025-01-13 09:29_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/test_functions.rs`:67 on 2025-01-13 09:29_

And PT028?

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/helpers.rs`:66 on 2025-01-13 09:44_

I still prefer checking for `test`, since that encompasses a superset of those encompassed by `test_`. I believe this is a useful divergence.

---

_@InSyncWithFoo reviewed on 2025-01-13 09:44_

---

_@InSyncWithFoo reviewed on 2025-01-13 09:47_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/helpers.rs`:63 on 2025-01-13 09:47_

Yes, but whether `is_likely_pytest_test_file()` should be called is what we are discussing. It does help with some false positives, as explained below.

---

_@MichaReiser reviewed on 2025-01-13 10:18_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/helpers.rs`:66 on 2025-01-13 10:18_

We should try to find the relevant code in pytest and then use whatever pytest uses.

---

_@MichaReiser reviewed on 2025-01-13 10:19_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/test_functions.rs`:78 on 2025-01-13 10:19_

We could make it `display-only` but I don't think we're displaying `display-only` fixes today, so we could as well just remove it. But I'm fine with either (display only or removing it)

---

_@InSyncWithFoo reviewed on 2025-01-13 11:39_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/helpers.rs`:66 on 2025-01-13 11:39_

Pytest [allows configuring naming conventions](https://docs.pytest.org/en/stable/example/pythoncollection.html#changing-naming-conventions). I suppose that means `test` should be used.

---

_@InSyncWithFoo reviewed on 2025-01-13 11:51_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/test_functions.rs`:67 on 2025-01-13 11:51_

The first `for` loop is for `PT019`. [Test cases](https://github.com/m-burst/flake8-pytest-style/blob/92c080312e36c13d2d7725d7bbd89d75d7eaae65/tests/test_PT028_test_function_argument_with_default.py) show that positional-only parameters are not ignored.

---

_@MichaReiser reviewed on 2025-01-13 12:26_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/helpers.rs`:66 on 2025-01-13 12:26_

We can keep `test` because of the default here https://github.com/pytest-dev/pytest/blob/477e9283aabc93f8d3ad34ff0b98f68b5a432aa8/src/_pytest/python.py#L96

---

_@MichaReiser approved on 2025-01-13 12:40_

Thanks for addressing the feedback. 

I'm leaning towards not adding the *is this a pytest file* just yet because it would introduce the need for a new option for those not using the default file name patterns. We can decide to restrict the rule in the future if this comes up often. The other reason why I'm for deferring is that there are still some false negatives because many of the non-test test functions are defined in files prefixed with `test_`

---

_Merged by @MichaReiser on 2025-01-13 12:40_

---

_Closed by @MichaReiser on 2025-01-13 12:40_

---

_Branch deleted on 2025-01-13 14:08_

---
