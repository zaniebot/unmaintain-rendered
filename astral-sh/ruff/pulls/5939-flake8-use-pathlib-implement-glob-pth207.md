```yaml
number: 5939
title: "[`flake8-use-pathlib`] Implement `glob` (`PTH207`)"
type: pull_request
state: merged
author: sbrugman
labels:
  - rule
assignees: []
merged: true
base: main
head: glob-pth207
created_at: 2023-07-21T02:48:33Z
updated_at: 2023-08-09T20:30:07Z
url: https://github.com/astral-sh/ruff/pull/5939
synced_at: 2026-01-12T02:52:03Z
```

# [`flake8-use-pathlib`] Implement `glob` (`PTH207`)

---

_Pull request opened by @sbrugman on 2023-07-21 02:48_

Discovered that the usage of `glob.glob` is [widespread](https://grep.app/search?current=7&q=glob.glob%28&filter%5Blang%5D%5B0%5D=Python) when working on the previous lints for `flake8-use-pathlib`.

---

_Comment by @github-actions[bot] on 2023-07-21 03:25_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+71, -0, 0 error(s))

<details><summary>airflow (+50, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/airflow/providers/google/cloud/transfers/local_to_gcs.py#L95'>airflow/providers/google/cloud/transfers/local_to_gcs.py:95:65:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/airflow/sensors/filesystem.py#L66'>airflow/sensors/filesystem.py:66:21:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/airflow/triggers/file.py#L64'>airflow/triggers/file.py:64:25:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/dev/breeze/src/airflow_breeze/utils/publish_docs_builder.py#L184'>dev/breeze/src/airflow_breeze/utils/publish_docs_builder.py:184:29:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/dev/breeze/src/airflow_breeze/utils/publish_docs_helpers.py#L57'>dev/breeze/src/airflow_breeze/utils/publish_docs_helpers.py:57:19:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/dev/provider_packages/prepare_provider_packages.py#L1793'>dev/provider_packages/prepare_provider_packages.py:1793:13:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/dev/provider_packages/prepare_provider_packages.py#L1796'>dev/provider_packages/prepare_provider_packages.py:1796:13:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/dev/provider_packages/remove_old_releases.py#L61'>dev/provider_packages/remove_old_releases.py:61:17:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/docker_tests/test_examples_of_prod_image_building.py#L48'>docker_tests/test_examples_of_prod_image_building.py:48:41:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/docker_tests/test_examples_of_prod_image_building.py#L53'>docker_tests/test_examples_of_prod_image_building.py:53:40:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/docs/exts/docs_build/dev_index_generator.py#L49'>docs/exts/docs_build/dev_index_generator.py:49:55:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/docs/exts/docs_build/docs_builder.py#L185'>docs/exts/docs_build/docs_builder.py:185:29:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/docs/exts/docs_build/lint_checks.py#L105'>docs/exts/docs_build/lint_checks.py:105:13:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/docs/exts/docs_build/lint_checks.py#L106'>docs/exts/docs_build/lint_checks.py:106:13:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/docs/exts/docs_build/lint_checks.py#L107'>docs/exts/docs_build/lint_checks.py:107:13:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/docs/exts/docs_build/lint_checks.py#L219'>docs/exts/docs_build/lint_checks.py:219:18:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/docs/exts/docs_build/lint_checks.py#L233'>docs/exts/docs_build/lint_checks.py:233:22:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/docs/exts/docs_build/lint_checks.py#L251'>docs/exts/docs_build/lint_checks.py:251:22:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/docs/exts/docs_build/lint_checks.py#L269'>docs/exts/docs_build/lint_checks.py:269:16:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/docs/exts/docs_build/lint_checks.py#L270'>docs/exts/docs_build/lint_checks.py:270:16:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/docs/exts/docs_build/lint_checks.py#L43'>docs/exts/docs_build/lint_checks.py:43:13:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/docs/exts/docs_build/lint_checks.py#L91'>docs/exts/docs_build/lint_checks.py:91:17:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/docs/exts/docs_build/lint_checks.py#L92'>docs/exts/docs_build/lint_checks.py:92:17:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/docs/exts/provider_yaml_utils.py#L54'>docs/exts/provider_yaml_utils.py:54:19:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/scripts/ci/pre_commit/pre_commit_check_deferrable_default.py#L89'>scripts/ci/pre_commit/pre_commit_check_deferrable_default.py:89:9:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/scripts/ci/pre_commit/pre_commit_check_deferrable_default.py#L90'>scripts/ci/pre_commit/pre_commit_check_deferrable_default.py:90:9:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/scripts/ci/pre_commit/pre_commit_check_providers_subpackages_all_have_init.py#L51'>scripts/ci/pre_commit/pre_commit_check_providers_subpackages_all_have_init.py:51:43:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/scripts/ci/pre_commit/pre_commit_check_providers_subpackages_all_have_init.py#L53'>scripts/ci/pre_commit/pre_commit_check_providers_subpackages_all_have_init.py:53:48:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/scripts/tools/list-integrations.py#L114'>scripts/tools/list-integrations.py:114:34:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/setup.py#L127'>setup.py:127:27:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/setup.py#L128'>setup.py:128:27:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/setup.py#L129'>setup.py:129:27:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/setup.py#L130'>setup.py:130:27:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/setup.py#L131'>setup.py:131:27:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/setup.py#L132'>setup.py:132:27:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/setup.py#L133'>setup.py:133:27:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/setup.py#L795'>setup.py:795:35:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/tests/always/test_example_dags.py#L60'>tests/always/test_example_dags.py:60:22:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/tests/always/test_project_structure.py#L128'>tests/always/test_project_structure.py:128:28:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/tests/always/test_project_structure.py#L174'>tests/always/test_project_structure.py:174:20:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/tests/always/test_project_structure.py#L178'>tests/always/test_project_structure.py:178:20:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/tests/always/test_project_structure.py#L34'>tests/always/test_project_structure.py:34:25:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/tests/always/test_project_structure.py#L40'>tests/always/test_project_structure.py:40:25:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/tests/always/test_project_structure.py#L460'>tests/always/test_project_structure.py:460:17:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/tests/always/test_project_structure.py#L61'>tests/always/test_project_structure.py:61:25:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/tests/always/test_project_structure.py#L80'>tests/always/test_project_structure.py:80:30:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/tests/providers/google/cloud/transfers/test_local_to_gcs.py#L123'>tests/providers/google/cloud/transfers/test_local_to_gcs.py:123:66:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/tests/providers/google/cloud/transfers/test_local_to_gcs.py#L124'>tests/providers/google/cloud/transfers/test_local_to_gcs.py:124:29:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/tests/providers/google/cloud/transfers/test_local_to_gcs.py#L147'>tests/providers/google/cloud/transfers/test_local_to_gcs.py:147:15:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/apache/airflow/blob/3237cb3484f0e7ec611ffba13360185e2f6c33f8/tests/serialization/test_dag_serialization.py#L315'>tests/serialization/test_dag_serialization.py:315:26:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
</pre>

</p>
</details>
<details><summary>bokeh (+4, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/scripts/sri.py#L56'>scripts/sri.py:56:25:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/src/bokeh/command/subcommands/serve.py#L824'>src/bokeh/command/subcommands/serve.py:824:30:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/src/bokeh/resources.py#L217'>src/bokeh/resources.py:217:13:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/tests/support/util/examples.py#L161'>tests/support/util/examples.py:161:24:</a> PTH207 Replace `iglob` with `Path.glob` or `Path.rglob`
</pre>

</p>
</details>
<details><summary>zulip (+17, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/3b63faf33cc4f10fa11036c3095266decf31365a/scripts/lib/clean_venv_cache.py#L43'>scripts/lib/clean_venv_cache.py:43:30:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/zulip/zulip/blob/3b63faf33cc4f10fa11036c3095266decf31365a/tools/lib/provision_inner.py#L323'>tools/lib/provision_inner.py:323:17:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/zulip/zulip/blob/3b63faf33cc4f10fa11036c3095266decf31365a/tools/lib/provision_inner.py#L70'>tools/lib/provision_inner.py:70:14:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/zulip/zulip/blob/3b63faf33cc4f10fa11036c3095266decf31365a/tools/lib/provision_inner.py#L71'>tools/lib/provision_inner.py:71:14:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/zulip/zulip/blob/3b63faf33cc4f10fa11036c3095266decf31365a/tools/lib/test_script.py#L115'>tools/lib/test_script.py:115:13:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/zulip/zulip/blob/3b63faf33cc4f10fa11036c3095266decf31365a/tools/lib/test_script.py#L115'>tools/lib/test_script.py:115:57:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/zulip/zulip/blob/3b63faf33cc4f10fa11036c3095266decf31365a/tools/lib/test_script.py#L127'>tools/lib/test_script.py:127:14:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/zulip/zulip/blob/3b63faf33cc4f10fa11036c3095266decf31365a/tools/setup/generate_zulip_bots_static_files.py#L39'>tools/setup/generate_zulip_bots_static_files.py:39:25:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/zulip/zulip/blob/3b63faf33cc4f10fa11036c3095266decf31365a/zerver/lib/dev_ldap_directory.py#L28'>zerver/lib/dev_ldap_directory.py:28:17:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/zulip/zulip/blob/3b63faf33cc4f10fa11036c3095266decf31365a/zerver/lib/export.py#L1768'>zerver/lib/export.py:1768:27:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/zulip/zulip/blob/3b63faf33cc4f10fa11036c3095266decf31365a/zerver/lib/export.py#L1803'>zerver/lib/export.py:1803:31:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/zulip/zulip/blob/3b63faf33cc4f10fa11036c3095266decf31365a/zerver/lib/export.py#L1868'>zerver/lib/export.py:1868:21:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/zulip/zulip/blob/3b63faf33cc4f10fa11036c3095266decf31365a/zerver/lib/test_fixtures.py#L341'>zerver/lib/test_fixtures.py:341:13:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/zulip/zulip/blob/3b63faf33cc4f10fa11036c3095266decf31365a/zerver/lib/test_fixtures.py#L382'>zerver/lib/test_fixtures.py:382:19:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/zulip/zulip/blob/3b63faf33cc4f10fa11036c3095266decf31365a/zerver/lib/test_fixtures.py#L65'>zerver/lib/test_fixtures.py:65:10:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/zulip/zulip/blob/3b63faf33cc4f10fa11036c3095266decf31365a/zerver/management/commands/export_usermessage_batch.py#L26'>zerver/management/commands/export_usermessage_batch.py:26:21:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
+ <a href='https://github.com/zulip/zulip/blob/3b63faf33cc4f10fa11036c3095266decf31365a/zerver/management/commands/makemessages.py#L231'>zerver/management/commands/makemessages.py:231:17:</a> PTH207 Replace `glob` with `Path.glob` or `Path.rglob`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PTH207 | 71 | 71 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.6±0.07ms     4.8 MB/sec    1.00      8.6±0.09ms     4.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1712.7±38.31µs     9.7 MB/sec    1.01  1737.0±31.61µs     9.6 MB/sec
formatter/numpy/globals.py                 1.00    190.9±5.55µs    15.5 MB/sec    1.03    196.0±4.25µs    15.1 MB/sec
formatter/pydantic/types.py                1.00      3.7±0.09ms     6.9 MB/sec    1.00      3.7±0.11ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.00     12.2±0.15ms     3.3 MB/sec    1.00     12.1±0.09ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.0±0.03ms     5.5 MB/sec    1.01      3.0±0.03ms     5.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    392.0±1.20µs     7.5 MB/sec    1.00    393.0±0.78µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.02      5.5±0.11ms     4.6 MB/sec    1.00      5.4±0.03ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.8±0.03ms     7.0 MB/sec    1.00      5.8±0.04ms     7.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1230.9±12.36µs    13.5 MB/sec    1.00  1231.9±13.44µs    13.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    134.7±4.29µs    21.9 MB/sec    1.01    135.6±4.00µs    21.8 MB/sec
linter/default-rules/pydantic/types.py     1.03      2.7±0.06ms     9.6 MB/sec    1.00      2.6±0.01ms     9.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.3±0.05ms     3.9 MB/sec    1.13     11.7±0.06ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   1960.5±9.08µs     8.5 MB/sec    1.11      2.2±0.01ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    204.3±1.94µs    14.4 MB/sec    1.09    221.8±9.25µs    13.3 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.04ms     5.8 MB/sec    1.11      4.9±0.03ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.4±0.05ms     2.8 MB/sec    1.01     14.5±0.07ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.02ms     4.4 MB/sec    1.00      3.8±0.02ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    385.3±6.73µs     7.7 MB/sec    1.00    386.9±4.89µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.03ms     3.9 MB/sec    1.01      6.6±0.06ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.03ms     5.4 MB/sec    1.01      7.6±0.03ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1487.2±8.34µs    11.2 MB/sec    1.00   1488.0±9.27µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    149.0±1.32µs    19.8 MB/sec    1.01    150.2±2.23µs    19.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.02ms     7.8 MB/sec    1.02      3.3±0.03ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @sbrugman on 2023-07-21 03:27_

Good harvest, violations seem correct 

---

_Comment by @konstin on 2023-07-21 12:54_

I like replacing `glob` with `pathlib` a lot, but i don't think they are equivalent, e.g. in the ruff project root:

```python
>>> from glob import glob
>>> len(glob("crates/**/*.py"))
2
>>> from pathlib import Path
>>> len(list(Path("crates").glob("**/*.py")))
3057
>>> len(list(Path().glob("crates/**/*.py")))
3057
```

---

_Comment by @sbrugman on 2023-07-22 11:33_

@konstin That's absolutely right. In the rule itself, I think it suffices to mention the differences that exist to make users aware that a fix is simple, but not trivial. Implementing autofix is doable, but needs to consider each case.

If we look at the [real-world usage](https://grep.app/search?current=8&q=glob.glob%28&filter%5Blang%5D%5B0%5D=Python) of `glob`, I'm convinced that within the context of using pathlib / `flake8-use-pathlib` is it appropriate to recommend switching.

There are a couple of differences between `glob` and `Path.glob` that do not make them equivalent, e.g.:

- **Hidden files**: Pathlib intentionally does not filter out hidden files, glob does [link](https://bugs.python.org/issue26096). In Python 3.11, `glob` gets the `include_hidden` parameter to match this behaviour.
- **Iterator**: `Path.glob` returns an iterator, just like `glob.iglob`
- **Working directory**: The glob functions have an argument `root_dir` that change the working dir (and thus the working dir is not included in the resulting path). To achieve the same with pathlib, use `Path.rglob`
- **globstar (`**`)**: Pathlib uses globstar only to match directories, not files. For `glob`, if `recursive=False`, `**` seems to be the same as doing `*`.

Each of the differences between `glob` with `Path.glob` can be easily mitigated when fixing:
- Filtering hidden files post-hoc in the iterator is simple (compared to adding them later), or by explicitly excluding hidden files and directories in the pattern e.g. see [this](https://stackoverflow.com/questions/67175881/skip-hidden-files-and-directories-in-pathlib-glob) post.
- Going from iterator to list is simply casting it with `list()`, this is also what the `glob` implementation does under the hood.
- Use `Path.rglob`. 

<details>

Previous solution before discovering `rglob`. Starting with Python 3.11, the `contextlib.chdir` can be used to achieve the same behaviour explicitly, e.g.
```python
from contextlib import chdir

with chdir(root_dir):
    print(list(Path().glob(pattern)))
```
For earlier versions this can be emulated:
```python
from contextlib import contextmanager
import os

@contextmanager
def chdir(file_path):
    curdir = os.getcwd()
    os.chdir(file_path)
    try:
        yield
    finally:
        os.chdir(curdir)
```

</details>

- If `recursive=False`, one can simply replace `**` with `*` in some cases. I'm not absolutely sure here what cases this wouldn't work, but probably modifying the pattern will make conversion possible.



---

_Comment by @sbrugman on 2023-07-22 13:49_

Extended the documentation and included `iglob`.

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_use_pathlib/rules/glob_rule.rs`:19 on 2023-07-23 10:17_

```suggestion
/// | Hidden files      | Excludes hidden files by default. From Python 3.11, the `include_hidden` keyword can used.                                                                                      | Includes hidden files by default.                                                                                                          |
```

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_use_pathlib/rules/glob_rule.rs`:22 on 2023-07-23 10:19_

```suggestion
/// | Globstar (`**`)   | `glob` requires the `recursive` flag to be set to `True` for the `**` pattern to match any files and zero or more directories, subdirectories and symbolic links to directories. | The `**` pattern in `Path.glob` means “this directory and all subdirectories, recursively”. In other words, it enables recursive globbing. |
```

---

_@konstin approved on 2023-07-23 10:20_

Thanks for writing up the differences!

---

_@konstin reviewed on 2023-07-24 09:07_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_use_pathlib/rules/glob_rule.rs`:22 on 2023-07-24 09:07_

@charliermarsh Do wide tables work with our documentation format?

---

_Label `rule` added by @charliermarsh on 2023-07-26 23:02_

---

_@charliermarsh reviewed on 2023-07-26 23:02_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_use_pathlib/rules/glob_rule.rs`:22 on 2023-07-26 23:02_

It looks good in MkDocs. It's not great on the CLI but I think it's acceptable.

---

_Merged by @charliermarsh on 2023-07-26 23:15_

---

_Closed by @charliermarsh on 2023-07-26 23:15_

---

_Branch deleted on 2023-08-09 20:30_

---
