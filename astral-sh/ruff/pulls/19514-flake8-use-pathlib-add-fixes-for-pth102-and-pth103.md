```yaml
number: 19514
title: "[`flake8-use-pathlib`] Add fixes for `PTH102` and `PTH103`"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: feat/pth-autofixes
created_at: 2025-07-23T17:36:22Z
updated_at: 2025-09-18T14:39:33Z
url: https://github.com/astral-sh/ruff/pull/19514
synced_at: 2026-01-10T17:40:28Z
```

# [`flake8-use-pathlib`] Add fixes for `PTH102` and `PTH103`

---

_Pull request opened by @chirizxc on 2025-07-23 17:36_

## Summary

Part of https://github.com/astral-sh/ruff/issues/2331

## Test Plan

<!-- How was it tested? -->
`cargo nextest run flake8_use_pathlib`

---

_Comment by @chirizxc on 2025-07-23 17:45_

@ntBre It's part of #19213 (check_os_pathlib_single_arg_calls)

---

_Comment by @github-actions[bot] on 2025-07-23 17:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +212 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +50 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/c33b43196924073d1d6251c585f27d322f7d16b1/airflow-core/tests/unit/utils/log/test_file_processor_handler.py#L105'>airflow-core/tests/unit/utils/log/test_file_processor_handler.py:105:9:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/apache/airflow/blob/c33b43196924073d1d6251c585f27d322f7d16b1/airflow-core/tests/unit/utils/log/test_file_processor_handler.py#L105'>airflow-core/tests/unit/utils/log/test_file_processor_handler.py:105:9:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
+ <a href='https://github.com/apache/airflow/blob/c33b43196924073d1d6251c585f27d322f7d16b1/airflow-core/tests/unit/utils/log/test_log_reader.py#L96'>airflow-core/tests/unit/utils/log/test_log_reader.py:96:9:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/apache/airflow/blob/c33b43196924073d1d6251c585f27d322f7d16b1/airflow-core/tests/unit/utils/log/test_log_reader.py#L96'>airflow-core/tests/unit/utils/log/test_log_reader.py:96:9:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
+ <a href='https://github.com/apache/airflow/blob/c33b43196924073d1d6251c585f27d322f7d16b1/airflow-core/tests/unit/utils/test_file.py#L106'>airflow-core/tests/unit/utils/test_file.py:106:9:</a> PTH102 [*] `os.mkdir()` should be replaced by `Path.mkdir()`
- <a href='https://github.com/apache/airflow/blob/c33b43196924073d1d6251c585f27d322f7d16b1/airflow-core/tests/unit/utils/test_file.py#L106'>airflow-core/tests/unit/utils/test_file.py:106:9:</a> PTH102 `os.mkdir()` should be replaced by `Path.mkdir()`
+ <a href='https://github.com/apache/airflow/blob/c33b43196924073d1d6251c585f27d322f7d16b1/airflow-core/tests/unit/utils/test_file.py#L164'>airflow-core/tests/unit/utils/test_file.py:164:9:</a> PTH102 [*] `os.mkdir()` should be replaced by `Path.mkdir()`
- <a href='https://github.com/apache/airflow/blob/c33b43196924073d1d6251c585f27d322f7d16b1/airflow-core/tests/unit/utils/test_file.py#L164'>airflow-core/tests/unit/utils/test_file.py:164:9:</a> PTH102 `os.mkdir()` should be replaced by `Path.mkdir()`
+ <a href='https://github.com/apache/airflow/blob/c33b43196924073d1d6251c585f27d322f7d16b1/airflow-ctl/src/airflowctl/api/client.py#L133'>airflow-ctl/src/airflowctl/api/client.py:133:9:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/apache/airflow/blob/c33b43196924073d1d6251c585f27d322f7d16b1/airflow-ctl/src/airflowctl/api/client.py#L133'>airflow-ctl/src/airflowctl/api/client.py:133:9:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
... 41 additional changes omitted for rule PTH103
... 40 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/a82e310600d098a4ae6c234c284ee11a0af72091/superset/initialization/__init__.py#L121'>superset/initialization/__init__.py:121:9:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/apache/superset/blob/a82e310600d098a4ae6c234c284ee11a0af72091/superset/initialization/__init__.py#L121'>superset/initialization/__init__.py:121:9:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
+ <a href='https://github.com/apache/superset/blob/a82e310600d098a4ae6c234c284ee11a0af72091/superset/initialization/__init__.py#L814'>superset/initialization/__init__.py:814:17:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/apache/superset/blob/a82e310600d098a4ae6c234c284ee11a0af72091/superset/initialization/__init__.py#L814'>superset/initialization/__init__.py:814:17:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
+ <a href='https://github.com/apache/superset/blob/a82e310600d098a4ae6c234c284ee11a0af72091/superset/utils/core.py#L1211'>superset/utils/core.py:1211:9:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/apache/superset/blob/a82e310600d098a4ae6c234c284ee11a0af72091/superset/utils/core.py#L1211'>superset/utils/core.py:1211:9:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +14 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/defaults.py#L114'>tests/support/defaults.py:114:5:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/defaults.py#L114'>tests/support/defaults.py:114:5:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/jupyter_notebook.py#L89'>tests/support/plugins/jupyter_notebook.py:89:9:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/jupyter_notebook.py#L89'>tests/support/plugins/jupyter_notebook.py:89:9:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/examples.py#L243'>tests/support/util/examples.py:243:9:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/examples.py#L243'>tests/support/util/examples.py:243:9:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/filesystem.py#L178'>tests/support/util/filesystem.py:178:1:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/filesystem.py#L178'>tests/support/util/filesystem.py:178:1:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/filesystem.py#L87'>tests/support/util/filesystem.py:87:17:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/filesystem.py#L87'>tests/support/util/filesystem.py:87:17:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -0 violations, +18 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/f0474eec7b86af726bb6b4cbcf2cd6677decca20/examples/bulk_import/example_bulkinsert_csv.py#L122'>examples/bulk_import/example_bulkinsert_csv.py:122:5:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/milvus-io/pymilvus/blob/f0474eec7b86af726bb6b4cbcf2cd6677decca20/examples/bulk_import/example_bulkinsert_csv.py#L122'>examples/bulk_import/example_bulkinsert_csv.py:122:5:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/f0474eec7b86af726bb6b4cbcf2cd6677decca20/examples/bulk_import/example_bulkinsert_csv.py#L127'>examples/bulk_import/example_bulkinsert_csv.py:127:9:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/milvus-io/pymilvus/blob/f0474eec7b86af726bb6b4cbcf2cd6677decca20/examples/bulk_import/example_bulkinsert_csv.py#L127'>examples/bulk_import/example_bulkinsert_csv.py:127:9:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/f0474eec7b86af726bb6b4cbcf2cd6677decca20/examples/bulk_import/example_bulkinsert_json.py#L166'>examples/bulk_import/example_bulkinsert_json.py:166:5:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/milvus-io/pymilvus/blob/f0474eec7b86af726bb6b4cbcf2cd6677decca20/examples/bulk_import/example_bulkinsert_json.py#L166'>examples/bulk_import/example_bulkinsert_json.py:166:5:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/f0474eec7b86af726bb6b4cbcf2cd6677decca20/examples/bulk_import/example_bulkinsert_json.py#L171'>examples/bulk_import/example_bulkinsert_json.py:171:9:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/milvus-io/pymilvus/blob/f0474eec7b86af726bb6b4cbcf2cd6677decca20/examples/bulk_import/example_bulkinsert_json.py#L171'>examples/bulk_import/example_bulkinsert_json.py:171:9:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/f0474eec7b86af726bb6b4cbcf2cd6677decca20/examples/bulk_import/example_bulkinsert_numpy.py#L124'>examples/bulk_import/example_bulkinsert_numpy.py:124:5:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/milvus-io/pymilvus/blob/f0474eec7b86af726bb6b4cbcf2cd6677decca20/examples/bulk_import/example_bulkinsert_numpy.py#L124'>examples/bulk_import/example_bulkinsert_numpy.py:124:5:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
... 8 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +124 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/scripts/lib/puppet_cache.py#L55'>scripts/lib/puppet_cache.py:55:5:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/scripts/lib/puppet_cache.py#L55'>scripts/lib/puppet_cache.py:55:5:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/scripts/lib/zulip_tools.py#L213'>scripts/lib/zulip_tools.py:213:5:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/scripts/lib/zulip_tools.py#L213'>scripts/lib/zulip_tools.py:213:5:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/scripts/lib/zulip_tools.py#L222'>scripts/lib/zulip_tools.py:222:13:</a> PTH102 [*] `os.mkdir()` should be replaced by `Path.mkdir()`
- <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/scripts/lib/zulip_tools.py#L222'>scripts/lib/zulip_tools.py:222:13:</a> PTH102 `os.mkdir()` should be replaced by `Path.mkdir()`
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/scripts/lib/zulip_tools.py#L284'>scripts/lib/zulip_tools.py:284:9:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/scripts/lib/zulip_tools.py#L284'>scripts/lib/zulip_tools.py:284:9:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/scripts/lib/zulip_tools.py#L646'>scripts/lib/zulip_tools.py:646:5:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/scripts/lib/zulip_tools.py#L646'>scripts/lib/zulip_tools.py:646:5:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/tools/lib/provision.py#L56'>tools/lib/provision.py:56:5:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/tools/lib/provision.py#L56'>tools/lib/provision.py:56:5:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/tools/lib/provision_inner.py#L62'>tools/lib/provision_inner.py:62:9:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/tools/lib/provision_inner.py#L62'>tools/lib/provision_inner.py:62:9:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/tools/lib/test_script.py#L126'>tools/lib/test_script.py:126:5:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
- <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/tools/lib/test_script.py#L126'>tools/lib/test_script.py:126:5:</a> PTH103 `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/tools/setup/generate_bots_integrations_static_files.py#L23'>tools/setup/generate_bots_integrations_static_files.py:23:5:</a> PTH103 [*] `os.makedirs()` should be replaced by `Path.mkdir(parents=True)`
... 102 additional changes omitted for rule PTH103
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/tools/setup/generate_landing_page_images.py#L26'>tools/setup/generate_landing_page_images.py:26:9:</a> PTH102 [*] `os.mkdir()` should be replaced by `Path.mkdir()`
- <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/tools/setup/generate_landing_page_images.py#L26'>tools/setup/generate_landing_page_images.py:26:9:</a> PTH102 `os.mkdir()` should be replaced by `Path.mkdir()`
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/zerver/management/commands/backup.py#L41'>zerver/management/commands/backup.py:41:13:</a> PTH102 [*] `os.mkdir()` should be replaced by `Path.mkdir()`
- <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/zerver/management/commands/backup.py#L41'>zerver/management/commands/backup.py:41:13:</a> PTH102 `os.mkdir()` should be replaced by `Path.mkdir()`
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/zerver/worker/base.py#L275'>zerver/worker/base.py:275:13:</a> PTH102 [*] `os.mkdir()` should be replaced by `Path.mkdir()`
- <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/zerver/worker/base.py#L275'>zerver/worker/base.py:275:13:</a> PTH102 `os.mkdir()` should be replaced by `Path.mkdir()`
... 101 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PTH103 | 200 | 0 | 0 | 200 | 0 |
| PTH102 | 12 | 0 | 0 | 12 | 0 |

</p>
</details>




---

_Review requested from @ntBre by @ntBre on 2025-07-23 17:55_

---

_Comment by @chirizxc on 2025-07-23 17:56_

I see I made a mistake somewhere

---

_Comment by @chirizxc on 2025-07-23 18:01_

Yes, it's because of a [condition](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs#L34) in `check_os_pathlib_single_arg_calls`

---

_Comment by @chirizxc on 2025-07-23 18:39_

now autofixes will be for all possible cases except `dir_fd`

---

_Comment by @chirizxc on 2025-07-24 11:33_

I think this PR can also include `PTH211`, there will be similar autofix

---

_Label `fixes` added by @ntBre on 2025-07-24 13:58_

---

_Label `preview` added by @ntBre on 2025-07-24 13:58_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/mod.rs`:32 on 2025-07-24 14:01_

Just a small formatting nit, but could we group these with the imports above?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:9 on 2025-07-24 14:05_

Another small formatting nit:

```suggestion
use ruff_diagnostics::{Applicability, Edit, Fix};
use ruff_macros::{ViolationMetadata, derive_message_formats};
use ruff_python_ast::ExprCall;
use ruff_text_size::Ranged;

use crate::checkers::ast::Checker;
use crate::importer::ImportRequest;
use crate::preview::is_fix_os_makedirs_enabled;
use crate::rules::flake8_use_pathlib::helpers::is_pathlib_path_call;
use crate::{FixAvailability, Violation};
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:77 on 2025-07-24 14:08_

Let's do an early return here if the fix is not enabled. That will make the diff smaller when we stabilize.

```suggestion
    if !is_fix_os_makedirs_enabled(checker.settings()) {
        return;
    }
```

It's a bit awkward with the name, but nicer in the future 🤷 

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:101 on 2025-07-24 14:15_

Could we use `add_argument` here, or will that not work with the other edits we need?

https://github.com/astral-sh/ruff/blob/d13228ab856f8cce47b3031cb2b4f2a35401e7eb/crates/ruff_linter/src/fix/edits.rs#L269-L275

The main thing I don't like about the current version is that it converts all of the arguments to keyword arguments, when that isn't strictly necessary. We might be able to use `add_argument` plus a replacement of just the attribute instead of the whole call:

```python
os.makedirs("./nested/directory")
^^^^^^^^^^^
instead of
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

I'm not sure that will work though.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_mkdir.rs`:11 on 2025-07-24 14:16_

Same suggestion as above, grouping "third-party" crates and then `crate::` imports:


```suggestion
use ruff_diagnostics::{Applicability, Edit, Fix};
use ruff_macros::{ViolationMetadata, derive_message_formats};
use ruff_python_ast::ExprCall;
use ruff_text_size::Ranged;

use crate::checkers::ast::Checker;
use crate::importer::ImportRequest;
use crate::preview::is_fix_os_mkdir_enabled;
use crate::rules::flake8_use_pathlib::helpers::{
    is_keyword_only_argument_non_default, is_pathlib_path_call,
};
use crate::{FixAvailability, Violation};
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_mkdir.rs`:89 on 2025-07-24 14:18_

Same as above, can we early return here?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_mkdir.rs`:112 on 2025-07-24 14:21_

I think we should include the parentheses in the `format!` calls below, then the `None` branch here can just be `String::new`, which shouldn't allocate. We could even do something like this in that case:

```rust
let mkdir_args = mode_str.map(|mode| format!("mode={mode}")).unwrap_or_default();
```

---

_@ntBre reviewed on 2025-07-24 14:32_

Thanks! I have a few suggestions here, but most of them are pretty minor, and this looks good overall. The main question is around  adding the `parents=True` argument.

---

_@chirizxc reviewed on 2025-07-24 16:28_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/mod.rs`:32 on 2025-07-24 16:28_

> Just a small formatting nit, but could we group these with the imports above?

I thought that's what rustfmt was supposed to do

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_mkdir.rs`:112 on 2025-07-24 16:53_

```rust
        let mkdir_args = call
            .arguments
            .find_argument_value("mode", 1)
            .map(|expr| format!("mode={}", checker.locator().slice(expr.range())))
            .unwrap_or_default();

        let replacement = if is_pathlib_path_call(checker, path) {
            if mkdir_args.is_empty() {
                format!("{path_code}.mkdir()")
            } else {
                format!("{path_code}.mkdir({mkdir_args})")
            }
        } else {
            if mkdir_args.is_empty() {
                format!("{binding}({path_code}).mkdir()")
            } else {
                format!("{binding}({path_code}).mkdir({mkdir_args})")
            }
        };
 ```
 
 it came out something like this

---

_@chirizxc reviewed on 2025-07-24 16:53_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_mkdir.rs`:112 on 2025-07-24 16:55_

I think it's safe to format an empty string into the parentheses, so you can probably collapse those `is_empty` checks! But that looks good otherwise.

---

_@ntBre reviewed on 2025-07-24 16:55_

---

_@chirizxc reviewed on 2025-07-24 17:08_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:101 on 2025-07-24 17:08_

I'm not sure about `add_argument`

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:79 on 2025-07-26 15:04_

Do we check the total number of the arguments anywhere? Because we're replacing the whole range, we could change the program's behavior if there are unexpected keyword arguments. For example, I added this test case:

```py
os.makedirs("directory", not_mode=777, something_else=3)
```

which got fixed to this:

```py
pathlib.Path("directory").mkdir(parents=True)
```

dropping the unknown arguments and changing the program's behavior (it would have crashed before).

It's a bit annoying, but I think we need to try to account for all of the arguments, or at the very least preserve them if they're already present.

I played a bit with `add_argument` and then `remove_argument` for the leftover `name` argument but couldn't get anything working. I think maybe we should just loop over all of the `arguments` and preserve all of them instead of looking specifically for `mode` and `exist_ok` down below. That should help to preserve existing behavior when unknown arguments are present. Otherwise we need to validate every argument before offering a fix.

This also applies to `os.mkdir` and possibly other PTH fixes.

---

_@ntBre reviewed on 2025-07-26 15:04_

Thanks! This looks good except for some more concerns about argument handling, sorry for not catching this earlier.

---

_@chirizxc reviewed on 2025-07-26 15:18_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:79 on 2025-07-26 15:18_

I don't think we should offer autofixes at all if the program as a whole doesn't work

---

_@chirizxc reviewed on 2025-07-26 15:21_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:79 on 2025-07-26 15:21_

but in general, I like it better when we just take the arguments and put them in.

---

_@ntBre reviewed on 2025-07-26 15:21_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:79 on 2025-07-26 15:21_

Agreed. We'll need to validate all the arguments then. Basically just make sure that there aren't any arguments present besides `name`, `mode` (optional), and `exist_ok` (optional), for `os.makedirs`, at least. I'm playing with an API to make this easier, but it's a pretty manual process currently.

---

_@chirizxc reviewed on 2025-07-26 15:41_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:79 on 2025-07-26 15:41_

but wait a minute, it was right from the beginning

---

_@chirizxc reviewed on 2025-07-26 15:42_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:79 on 2025-07-26 15:42_

If we put the first argument `parents=True`, we cannot put positional argument further, because `SyntaxError: positional argument follows keyword argument`

---

_@chirizxc reviewed on 2025-07-26 15:43_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:79 on 2025-07-26 15:43_

<img width="1299" height="752" alt="изображение" src="https://github.com/user-attachments/assets/f4f7ef70-3c10-40a8-adcf-c3de928f6939" />


---

_@ntBre reviewed on 2025-07-26 16:20_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:79 on 2025-07-26 16:20_

Yes, but in this case we would want to add `parents=True` at the end of the argument list. You might want to take a look at the implementation of `add_argument` even if we can't use it directly here.

I think we've kind of mixed two of my concerns together:
- I don't want to convert _all_ of the arguments to keywords unnecessarily. I'm assuming the input code for your screenshot here was:
  ```py
  import os

  os.makedirs("name", 0o777, False)
  ```

  This should be fixed to
  
  ```py
  pathlib.Path("name").mkdir(0o777, False, parents=True)
  ```
  
  not
  
  ```py
  pathlib.Path("name").mkdir(mode=0o777, exist_ok=False, parents=True)
  ```
  
  or similar, which is what I brought up in https://github.com/astral-sh/ruff/pull/19514#discussion_r2228655881.
- We also need to validate the full argument list before offering a fix. That's what I brought up in this thread. Again, this code:
  ```python
  os.makedirs("directory", not_mode=777, something_else=3)
  ```
  
  should not get fixed instead of being fixed to
  
  ```python
  pathlib.Path("directory").mkdir(parents=True)
  ```
  
  which is currently the case and drops the unknown keyword arguments.

In short, I want us to be able to pass these two new test cases:

```python
os.makedirs("name", 0o777, False)  # should be fixed to pathlib.Path("name").mkdir(0o777, False, parents=True)
os.makedirs("name", unknown_kwarg=True)  # diagnostic but no fix, unknown arguments
```


---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:79 on 2025-07-26 19:14_

```python
os.makedirs("name", 0o777, False)  # should be fixed to pathlib.Path("name").mkdir(0o777, False, parents=True)
```
```
Traceback (most recent call last):
  File "C:\Users\chiri\Desktop\ruff_test\pth103.py", line 3, in <module>
    Path("name").mkdir(0o777, False, parents=True)
    ~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^
TypeError: Path.mkdir() got multiple values for argument 'parents'
```


---

_@chirizxc reviewed on 2025-07-26 19:14_

---

_@chirizxc reviewed on 2025-07-26 19:16_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:79 on 2025-07-26 19:16_

```rust
/// PTH103
pub(crate) fn os_makedirs(checker: &Checker, call: &ExprCall, segments: &[&str]) {
    if segments != ["os", "makedirs"] {
        return;
    }

    let range = call.range();
    let mut diagnostic = checker.report_diagnostic(OsMakedirs, call.func.range());

    let Some(name) = call.arguments.find_argument_value("name", 0) else {
        return;
    };

    if !is_fix_os_makedirs_enabled(checker.settings()) {
        return;
    }

    if call.arguments.args.len() > 3 {
        return;
    }

    if call.arguments.keywords.iter().any(|kw| {
        !matches!(kw.arg.as_deref(), Some("name" | "mode" | "exist_ok"))
    }) {
        return;
    }


    diagnostic.try_set_fix(|| {
        let (import_edit, binding) = checker.importer().get_or_import_symbol(
            &ImportRequest::import("pathlib", "Path"),
            call.start(),
            checker.semantic(),
        )?;

        let applicability = if checker.comment_ranges().intersects(range) {
            Applicability::Unsafe
        } else {
            Applicability::Safe
        };

        let name_code = checker.locator().slice(name.range());

        let args = call
            .arguments
            .args
            .iter()
            .skip(1)
            .map(|expr| checker.locator().slice(expr.range()).to_string())
            .chain(
                call.arguments.keywords.iter().filter_map(|kw| {
                    kw.arg.as_ref().and_then(|arg| {
                        if arg == "name" {
                            None
                        } else {
                            Some(format!(
                                "{arg}={}",
                                checker.locator().slice(kw.value.range())
                            ))
                        }
                    })
                }),
            )
            .chain(std::iter::once("parents=True".to_string()))
            .collect::<Vec<_>>();

        let mkdir_args = args.join(", ");

        let replacement = if is_pathlib_path_call(checker, name) {
            format!("{name_code}.mkdir({mkdir_args})")
        } else {
            format!("{binding}({name_code}).mkdir({mkdir_args})")
        };

        Ok(Fix::applicable_edits(
            Edit::range_replacement(replacement, range),
            [import_edit],
            applicability,
        ))
    });
}
```

I got something like this at the moment, I need to think about optimization and the case where all arguments are positional

---

_Comment by @chirizxc on 2025-07-26 19:59_

in this variant only for the case when all arguments are positional fix will make them named to avoid the problem with `TypeError: Path.mkdir() got multiple values for argument ‘parents’`

---

_Comment by @chirizxc on 2025-07-26 20:02_

Maybe there are more cases that I didn't see, I'll try to see more

---

_@chirizxc reviewed on 2025-07-28 18:43_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:121 on 2025-07-28 18:43_

```rust
            itertools::join(
                call.arguments
                    .args
                    .iter()
                    .skip(1)
                    .map(|expr| checker.locator().slice(expr.range()).to_string())
                    .chain(call.arguments.keywords.iter().filter_map(|kw| {
                        kw.arg.as_ref().and_then(|arg| {
                            (arg != "name").then(|| {
                                format!("{arg}={}", checker.locator().slice(kw.value.range()))
                            })
                        })
                    }))
                    .chain(std::iter::once("parents=True".to_string())),
                ", ",
            )
        };
```
I think that should be better

---

_Review requested from @ntBre by @chirizxc on 2025-08-01 15:38_

---

_Comment by @chirizxc on 2025-08-01 15:45_

We have only two autofix outputs, when all args positional:
1) already used in the code
```python
os.makedirs("name", 0o777, False) # => 
pathlib.Path("name").mkdir(mode=0o777, exist_ok=False, parents=True)
```
2) 
```python
os.makedirs("name", 0o777, False) # =>
pathlib.Path("name").mkdir(0o777, True, False)
```

---

_Comment by @chirizxc on 2025-08-06 18:22_

```python
# ruff: noqa: FBT003
import os

os.makedirs("name", True)
os.makedirs("name", 0o777)
os.makedirs("name", 0o777, True)
os.makedirs("name", 0o777, exist_ok=True)
os.makedirs("name", mode=0o777, exist_ok=True)
os.makedirs(name="name", mode=0o777, exist_ok=True)
os.makedirs("name", exist_ok=True, mode=0o777)

os.makedirs("name", idk="123")
os.makedirs("name", mode=0o777, exist_ok=False, x=True)
```
after fix:
```python
# ruff: noqa: FBT003
import os
from pathlib import Path

Path("name").mkdir(True, parents=True)
Path("name").mkdir(0o777, parents=True)
Path("name").mkdir(0o777, True, True)
Path("name").mkdir(0o777, exist_ok=True, parents=True)
Path("name").mkdir(mode=0o777, exist_ok=True, parents=True)
Path("name").mkdir(mode=0o777, exist_ok=True, parents=True)
Path("name").mkdir(exist_ok=True, mode=0o777, parents=True)

os.makedirs("name", idk="123")
os.makedirs("name", mode=0o777, exist_ok=False, x=True)
```

---

_Comment by @chirizxc on 2025-08-12 17:01_

@ntBre can you check this realization please?


---

_Comment by @ntBre on 2025-08-12 17:21_

Yes, sorry, this has been on my todo list. Do you have a question about the implementation or is this just ready for review again?

And sorry for all the snapshot conflicts, we just updated our diagnostic rendering. I _think_ you should just need to accept the new versions of the snapshots after re-running the tests.

---

_Comment by @chirizxc on 2025-08-12 17:24_

> Yes, sorry, this has been on my todo list. Do you have a question about the implementation or is this just ready for review again?
> 
> And sorry for all the snapshot conflicts, we just updated our diagnostic rendering. I _think_ you should just need to accept the new versions of the snapshots after re-running the tests.

it's ready

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:92 on 2025-08-19 19:51_

I think we could check `call.arguments.len()` instead of `arguments.args`. That will check both the positional (`.args`) and keyword (`.keywords`) arguments.

Another common way to run into trouble is with `*args` or `**kwargs`. We should probably make the fix unsafe or bail out without a fix if either of those are present. I think the only way to check for these is if `arguments.args.iter().any(|arg| arg.is_starred_expr())` or if `arguments.keywords.iter().any(|keyword| keyword.arg.is_none())`, respectively.

Again, sorry for the trouble. I wish there were a better API for this, but we've also been getting a lot of bug reports around these, so they're on my mind.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:137 on 2025-08-19 20:42_

What do you think about this? It feels a little simpler to me, but I believe it still covers all of the same cases:


```suggestion
        let mode = call.arguments.find_argument("mode", 1);
        let exist_ok = call.arguments.find_argument("exist_ok", 2);

        let locator = checker.locator();
        let mkdir_args = match (mode, exist_ok) {
            // Default to a keyword argument when alone.
            (None, None) => "parents=True".to_string(),
            // If either argument is missing, it's safe to add `parents` at the end.
            (None, Some(arg)) | (Some(arg), None) => {
                format!("{}, parents=True", locator.slice(arg))
            }
            // If they're all positional, `parents` has to be positional too.
            (Some(ArgOrKeyword::Arg(mode)), Some(ArgOrKeyword::Arg(exist_ok))) => {
                format!("{}, True, {}", locator.slice(mode), locator.slice(exist_ok))
            }
            // If either argument is a keyword, we can put `parents` at the end again.
            (Some(mode), Some(exist_ok)) => format!(
                "{}, {}, parents=True",
                locator.slice(mode),
                locator.slice(exist_ok)
            ),
        };
```

---

_@ntBre reviewed on 2025-08-19 20:48_

Thank you, this is looking good. I had a couple more, smaller argument validation suggestions and a potential simplification for the `parents` argument addition. Thanks for handling all of my pedantic feedback here, these are going to be great fixes :)

---

_@ntBre approved on 2025-08-20 18:35_

Thank you!

---

_Renamed from "[`flake8-use-pathlib`] Add autofix for `PTH102`, `PTH103`" to "[`flake8-use-pathlib`] Add fixes for `PTH102` and `PTH103`" by @ntBre on 2025-08-20 18:35_

---

_Merged by @ntBre on 2025-08-20 18:36_

---

_Closed by @ntBre on 2025-08-20 18:36_

---

_Branch deleted on 2025-09-18 14:39_

---
