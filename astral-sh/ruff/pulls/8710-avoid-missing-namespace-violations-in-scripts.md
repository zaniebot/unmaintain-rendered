```yaml
number: 8710
title: Avoid missing namespace violations in scripts with shebangs
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/scripts
created_at: 2023-11-16T02:23:32Z
updated_at: 2023-11-16T22:21:35Z
url: https://github.com/astral-sh/ruff/pull/8710
synced_at: 2026-01-10T23:40:55Z
```

# Avoid missing namespace violations in scripts with shebangs

---

_Pull request opened by @charliermarsh on 2023-11-16 02:23_

## Summary

I think it's reasonable to avoid raising `INP001` for scripts, and shebangs are one sufficient way to detect scripts.

Closes https://github.com/astral-sh/ruff/issues/8690.


---

_Review requested from @zanieb by @charliermarsh on 2023-11-16 02:23_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-11-16 02:23_

---

_Label `rule` added by @charliermarsh on 2023-11-16 02:23_

---

_Label `rule` removed by @charliermarsh on 2023-11-16 02:23_

---

_Label `bug` added by @charliermarsh on 2023-11-16 02:23_

---

_Comment by @github-actions[bot] on 2023-11-16 02:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -79 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -75 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/dev/chart/build_changelog_annotations.py#L1'>dev/chart/build_changelog_annotations.py:1:1:</a> INP001 File `dev/chart/build_changelog_annotations.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/dev/example_dags/update_example_dags_paths.py#L1'>dev/example_dags/update_example_dags_paths.py:1:1:</a> INP001 File `dev/example_dags/update_example_dags_paths.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/dev/stats/calculate_statistics_provider_testing_issues.py#L1'>dev/stats/calculate_statistics_provider_testing_issues.py:1:1:</a> INP001 File `dev/stats/calculate_statistics_provider_testing_issues.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/dev/stats/get_important_pr_candidates.py#L1'>dev/stats/get_important_pr_candidates.py:1:1:</a> INP001 File `dev/stats/get_important_pr_candidates.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/dev/system_tests/update_issue_status.py#L1'>dev/system_tests/update_issue_status.py:1:1:</a> INP001 File `dev/system_tests/update_issue_status.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_base_operator_partial_arguments.py#L1'>scripts/ci/pre_commit/pre_commit_base_operator_partial_arguments.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_base_operator_partial_arguments.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_boring_cyborg.py#L1'>scripts/ci/pre_commit/pre_commit_boring_cyborg.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_boring_cyborg.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_breeze_cmd_line.py#L1'>scripts/ci/pre_commit/pre_commit_breeze_cmd_line.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_breeze_cmd_line.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_changelog_duplicates.py#L1'>scripts/ci/pre_commit/pre_commit_changelog_duplicates.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_changelog_duplicates.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_chart_schema.py#L1'>scripts/ci/pre_commit/pre_commit_chart_schema.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_chart_schema.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_aiobotocore_optional.py#L1'>scripts/ci/pre_commit/pre_commit_check_aiobotocore_optional.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_aiobotocore_optional.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_airflow_k8s_not_used.py#L1'>scripts/ci/pre_commit/pre_commit_check_airflow_k8s_not_used.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_airflow_k8s_not_used.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_cncf_k8s_used_for_k8s_executor_only.py#L1'>scripts/ci/pre_commit/pre_commit_check_cncf_k8s_used_for_k8s_executor_only.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_cncf_k8s_used_for_k8s_executor_only.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_deferrable_default.py#L1'>scripts/ci/pre_commit/pre_commit_check_deferrable_default.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_deferrable_default.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_google_re2_imports.py#L1'>scripts/ci/pre_commit/pre_commit_check_google_re2_imports.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_google_re2_imports.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_init_in_tests.py#L1'>scripts/ci/pre_commit/pre_commit_check_init_in_tests.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_init_in_tests.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_lazy_logging.py#L1'>scripts/ci/pre_commit/pre_commit_check_lazy_logging.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_lazy_logging.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_license.py#L1'>scripts/ci/pre_commit/pre_commit_check_license.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_license.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_order_dockerfile_extras.py#L1'>scripts/ci/pre_commit/pre_commit_check_order_dockerfile_extras.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_order_dockerfile_extras.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_order_setup.py#L1'>scripts/ci/pre_commit/pre_commit_check_order_setup.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_order_setup.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_pre_commit_hooks.py#L1'>scripts/ci/pre_commit/pre_commit_check_pre_commit_hooks.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_pre_commit_hooks.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_provider_airflow_compatibility.py#L1'>scripts/ci/pre_commit/pre_commit_check_provider_airflow_compatibility.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_provider_airflow_compatibility.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_provider_docs.py#L1'>scripts/ci/pre_commit/pre_commit_check_provider_docs.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_provider_docs.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_provider_yaml_files.py#L1'>scripts/ci/pre_commit/pre_commit_check_provider_yaml_files.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_provider_yaml_files.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_providers_init.py#L1'>scripts/ci/pre_commit/pre_commit_check_providers_init.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_providers_init.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_providers_subpackages_all_have_init.py#L1'>scripts/ci/pre_commit/pre_commit_check_providers_subpackages_all_have_init.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_providers_subpackages_all_have_init.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_setup_extra_packages_ref.py#L1'>scripts/ci/pre_commit/pre_commit_check_setup_extra_packages_ref.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_setup_extra_packages_ref.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_system_tests.py#L1'>scripts/ci/pre_commit/pre_commit_check_system_tests.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_system_tests.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_system_tests_hidden_in_index.py#L1'>scripts/ci/pre_commit/pre_commit_check_system_tests_hidden_in_index.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_system_tests_hidden_in_index.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_checkout_no_credentials.py#L1'>scripts/ci/pre_commit/pre_commit_checkout_no_credentials.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_checkout_no_credentials.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_compile_www_assets.py#L1'>scripts/ci/pre_commit/pre_commit_compile_www_assets.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_compile_www_assets.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_compile_www_assets_dev.py#L1'>scripts/ci/pre_commit/pre_commit_compile_www_assets_dev.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_compile_www_assets_dev.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_decorator_operator_implements_custom_name.py#L1'>scripts/ci/pre_commit/pre_commit_decorator_operator_implements_custom_name.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_decorator_operator_implements_custom_name.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_docstring_param_type.py#L1'>scripts/ci/pre_commit/pre_commit_docstring_param_type.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_docstring_param_type.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_generate_pypi_readme.py#L1'>scripts/ci/pre_commit/pre_commit_generate_pypi_readme.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_generate_pypi_readme.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_helm_lint.py#L1'>scripts/ci/pre_commit/pre_commit_helm_lint.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_helm_lint.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_inline_scripts_in_docker.py#L1'>scripts/ci/pre_commit/pre_commit_inline_scripts_in_docker.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_inline_scripts_in_docker.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_insert_extras.py#L1'>scripts/ci/pre_commit/pre_commit_insert_extras.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_insert_extras.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_json_schema.py#L1'>scripts/ci/pre_commit/pre_commit_json_schema.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_json_schema.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_lint_dockerfile.py#L1'>scripts/ci/pre_commit/pre_commit_lint_dockerfile.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_lint_dockerfile.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_local_yml_mounts.py#L1'>scripts/ci/pre_commit/pre_commit_local_yml_mounts.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_local_yml_mounts.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_migration_reference.py#L1'>scripts/ci/pre_commit/pre_commit_migration_reference.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_migration_reference.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_mypy.py#L1'>scripts/ci/pre_commit/pre_commit_mypy.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_mypy.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_new_session_in_provide_session.py#L1'>scripts/ci/pre_commit/pre_commit_new_session_in_provide_session.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_new_session_in_provide_session.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_newsfragments.py#L1'>scripts/ci/pre_commit/pre_commit_newsfragments.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_newsfragments.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_replace_bad_characters.py#L1'>scripts/ci/pre_commit/pre_commit_replace_bad_characters.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_replace_bad_characters.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_sort_in_the_wild.py#L1'>scripts/ci/pre_commit/pre_commit_sort_in_the_wild.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_sort_in_the_wild.py` is part of an implicit namespace package. Add an `__init__.py`.
... 28 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/output/apis/server_document/bokeh_server.py#L1'>examples/output/apis/server_document/bokeh_server.py:1:1:</a> INP001 File `examples/output/apis/server_document/bokeh_server.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/scripts/milestone.py#L1'>scripts/milestone.py:1:1:</a> INP001 File `scripts/milestone.py` is part of an implicit namespace package. Add an `__init__.py`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/c5909c0fe9a4e1c24720c652807b84be7fd7831a/.github/scripts/notifier.py#L1'>.github/scripts/notifier.py:1:1:</a> INP001 File `.github/scripts/notifier.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/rotki/rotki/blob/c5909c0fe9a4e1c24720c652807b84be7fd7831a/packaging/docker/entrypoint.py#L1'>packaging/docker/entrypoint.py:1:1:</a> INP001 File `packaging/docker/entrypoint.py` is part of an implicit namespace package. Add an `__init__.py`.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| INP001 | 79 | 0 | 79 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -79 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -75 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/dev/chart/build_changelog_annotations.py#L1'>dev/chart/build_changelog_annotations.py:1:1:</a> INP001 File `dev/chart/build_changelog_annotations.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/dev/example_dags/update_example_dags_paths.py#L1'>dev/example_dags/update_example_dags_paths.py:1:1:</a> INP001 File `dev/example_dags/update_example_dags_paths.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/dev/stats/calculate_statistics_provider_testing_issues.py#L1'>dev/stats/calculate_statistics_provider_testing_issues.py:1:1:</a> INP001 File `dev/stats/calculate_statistics_provider_testing_issues.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/dev/stats/get_important_pr_candidates.py#L1'>dev/stats/get_important_pr_candidates.py:1:1:</a> INP001 File `dev/stats/get_important_pr_candidates.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/dev/system_tests/update_issue_status.py#L1'>dev/system_tests/update_issue_status.py:1:1:</a> INP001 File `dev/system_tests/update_issue_status.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_base_operator_partial_arguments.py#L1'>scripts/ci/pre_commit/pre_commit_base_operator_partial_arguments.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_base_operator_partial_arguments.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_boring_cyborg.py#L1'>scripts/ci/pre_commit/pre_commit_boring_cyborg.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_boring_cyborg.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_breeze_cmd_line.py#L1'>scripts/ci/pre_commit/pre_commit_breeze_cmd_line.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_breeze_cmd_line.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_changelog_duplicates.py#L1'>scripts/ci/pre_commit/pre_commit_changelog_duplicates.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_changelog_duplicates.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_chart_schema.py#L1'>scripts/ci/pre_commit/pre_commit_chart_schema.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_chart_schema.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_aiobotocore_optional.py#L1'>scripts/ci/pre_commit/pre_commit_check_aiobotocore_optional.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_aiobotocore_optional.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_airflow_k8s_not_used.py#L1'>scripts/ci/pre_commit/pre_commit_check_airflow_k8s_not_used.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_airflow_k8s_not_used.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_cncf_k8s_used_for_k8s_executor_only.py#L1'>scripts/ci/pre_commit/pre_commit_check_cncf_k8s_used_for_k8s_executor_only.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_cncf_k8s_used_for_k8s_executor_only.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_deferrable_default.py#L1'>scripts/ci/pre_commit/pre_commit_check_deferrable_default.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_deferrable_default.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_google_re2_imports.py#L1'>scripts/ci/pre_commit/pre_commit_check_google_re2_imports.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_google_re2_imports.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_init_in_tests.py#L1'>scripts/ci/pre_commit/pre_commit_check_init_in_tests.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_init_in_tests.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_lazy_logging.py#L1'>scripts/ci/pre_commit/pre_commit_check_lazy_logging.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_lazy_logging.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_license.py#L1'>scripts/ci/pre_commit/pre_commit_check_license.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_license.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_order_dockerfile_extras.py#L1'>scripts/ci/pre_commit/pre_commit_check_order_dockerfile_extras.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_order_dockerfile_extras.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_order_setup.py#L1'>scripts/ci/pre_commit/pre_commit_check_order_setup.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_order_setup.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_pre_commit_hooks.py#L1'>scripts/ci/pre_commit/pre_commit_check_pre_commit_hooks.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_pre_commit_hooks.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_provider_airflow_compatibility.py#L1'>scripts/ci/pre_commit/pre_commit_check_provider_airflow_compatibility.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_provider_airflow_compatibility.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_provider_docs.py#L1'>scripts/ci/pre_commit/pre_commit_check_provider_docs.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_provider_docs.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_provider_yaml_files.py#L1'>scripts/ci/pre_commit/pre_commit_check_provider_yaml_files.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_provider_yaml_files.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_providers_init.py#L1'>scripts/ci/pre_commit/pre_commit_check_providers_init.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_providers_init.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_providers_subpackages_all_have_init.py#L1'>scripts/ci/pre_commit/pre_commit_check_providers_subpackages_all_have_init.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_providers_subpackages_all_have_init.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_setup_extra_packages_ref.py#L1'>scripts/ci/pre_commit/pre_commit_check_setup_extra_packages_ref.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_setup_extra_packages_ref.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_system_tests.py#L1'>scripts/ci/pre_commit/pre_commit_check_system_tests.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_system_tests.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_check_system_tests_hidden_in_index.py#L1'>scripts/ci/pre_commit/pre_commit_check_system_tests_hidden_in_index.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_check_system_tests_hidden_in_index.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_checkout_no_credentials.py#L1'>scripts/ci/pre_commit/pre_commit_checkout_no_credentials.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_checkout_no_credentials.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_compile_www_assets.py#L1'>scripts/ci/pre_commit/pre_commit_compile_www_assets.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_compile_www_assets.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_compile_www_assets_dev.py#L1'>scripts/ci/pre_commit/pre_commit_compile_www_assets_dev.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_compile_www_assets_dev.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_decorator_operator_implements_custom_name.py#L1'>scripts/ci/pre_commit/pre_commit_decorator_operator_implements_custom_name.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_decorator_operator_implements_custom_name.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_docstring_param_type.py#L1'>scripts/ci/pre_commit/pre_commit_docstring_param_type.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_docstring_param_type.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_generate_pypi_readme.py#L1'>scripts/ci/pre_commit/pre_commit_generate_pypi_readme.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_generate_pypi_readme.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_helm_lint.py#L1'>scripts/ci/pre_commit/pre_commit_helm_lint.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_helm_lint.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_inline_scripts_in_docker.py#L1'>scripts/ci/pre_commit/pre_commit_inline_scripts_in_docker.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_inline_scripts_in_docker.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_insert_extras.py#L1'>scripts/ci/pre_commit/pre_commit_insert_extras.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_insert_extras.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_json_schema.py#L1'>scripts/ci/pre_commit/pre_commit_json_schema.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_json_schema.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_lint_dockerfile.py#L1'>scripts/ci/pre_commit/pre_commit_lint_dockerfile.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_lint_dockerfile.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_local_yml_mounts.py#L1'>scripts/ci/pre_commit/pre_commit_local_yml_mounts.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_local_yml_mounts.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_migration_reference.py#L1'>scripts/ci/pre_commit/pre_commit_migration_reference.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_migration_reference.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_mypy.py#L1'>scripts/ci/pre_commit/pre_commit_mypy.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_mypy.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_new_session_in_provide_session.py#L1'>scripts/ci/pre_commit/pre_commit_new_session_in_provide_session.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_new_session_in_provide_session.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_newsfragments.py#L1'>scripts/ci/pre_commit/pre_commit_newsfragments.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_newsfragments.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_replace_bad_characters.py#L1'>scripts/ci/pre_commit/pre_commit_replace_bad_characters.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_replace_bad_characters.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/apache/airflow/blob/e29464b06299a339e8125385c9c32a04e119f8ee/scripts/ci/pre_commit/pre_commit_sort_in_the_wild.py#L1'>scripts/ci/pre_commit/pre_commit_sort_in_the_wild.py:1:1:</a> INP001 File `scripts/ci/pre_commit/pre_commit_sort_in_the_wild.py` is part of an implicit namespace package. Add an `__init__.py`.
... 28 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/output/apis/server_document/bokeh_server.py#L1'>examples/output/apis/server_document/bokeh_server.py:1:1:</a> INP001 File `examples/output/apis/server_document/bokeh_server.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/scripts/milestone.py#L1'>scripts/milestone.py:1:1:</a> INP001 File `scripts/milestone.py` is part of an implicit namespace package. Add an `__init__.py`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/c5909c0fe9a4e1c24720c652807b84be7fd7831a/.github/scripts/notifier.py#L1'>.github/scripts/notifier.py:1:1:</a> INP001 File `.github/scripts/notifier.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/rotki/rotki/blob/c5909c0fe9a4e1c24720c652807b84be7fd7831a/packaging/docker/entrypoint.py#L1'>packaging/docker/entrypoint.py:1:1:</a> INP001 File `packaging/docker/entrypoint.py` is part of an implicit namespace package. Add an `__init__.py`.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| INP001 | 79 | 0 | 79 | 0 | 0 |

</p>
</details>




---

_Comment by @zanieb on 2023-11-16 03:00_

Is this a bug fix? Arguably this is a significant change in scope and should be gated by preview. The ecosystem results suggest this will affect people. I'm okay with it since it is a reasonable reduction in scope, but it seems worth raising.

---

_@zanieb approved on 2023-11-16 03:00_

---

_Comment by @sanmai-NL on 2023-11-16 06:59_

The ecosystem changes are clear fixes, right? The impact isn't breakage but less breakage. How does that influence your preview process?

---

_Comment by @charliermarsh on 2023-11-16 22:21_

Good callout, although I think this is okay because it's a reduction in scope.

---

_Merged by @charliermarsh on 2023-11-16 22:21_

---

_Closed by @charliermarsh on 2023-11-16 22:21_

---

_Branch deleted on 2023-11-16 22:21_

---
