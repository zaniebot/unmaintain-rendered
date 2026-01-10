```yaml
number: 17268
title: "Control flow: Switch statements"
type: pull_request
state: closed
author: dylwil3
labels:
  - internal
  - do-not-merge
assignees: []
draft: true
base: main
head: cfg-switch
created_at: 2025-04-07T07:45:36Z
updated_at: 2025-12-10T18:00:36Z
url: https://github.com/astral-sh/ruff/pull/17268
synced_at: 2026-01-10T16:42:11Z
```

# Control flow: Switch statements

---

_Pull request opened by @dylwil3 on 2025-04-07 07:45_

This PR implements switch statements in the control flow graph builder.

There are a few false positives in the ecosystem check because we have not implemented the fallback for exception handling statements, but otherwise things are looking good.

Note: This is currently based on the `cfg-loops` branch, so I will have to rebase after it's merged in. But if you want a head start on reviewing, just start with commit [8d67bf3](https://github.com/astral-sh/ruff/pull/17268/commits/8d67bf3c193d2118c7cb5eab28650a26da90a3b4).


---

_Comment by @github-actions[bot] on 2025-04-07 07:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+20 -1 violations, +0 -0 fixes in 12 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/state.py#L2531'>disnake/state.py:2531:9:</a> PLW0101 Unreachable code in `_delay_ready`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/42024188b9553dd50072e717af754cb95c901972/airflow-core/src/airflow/models/dag.py#L2581'>airflow-core/src/airflow/models/dag.py:2581:5:</a> PLW0101 Unreachable code in `_run_task`
+ <a href='https://github.com/apache/airflow/blob/42024188b9553dd50072e717af754cb95c901972/airflow-core/src/airflow/triggers/testing.py#L52'>airflow-core/src/airflow/triggers/testing.py:52:13:</a> PLW0101 Unreachable code in `run`
+ <a href='https://github.com/apache/airflow/blob/42024188b9553dd50072e717af754cb95c901972/providers/sftp/src/airflow/providers/sftp/triggers/sftp.py#L138'>providers/sftp/src/airflow/providers/sftp/triggers/sftp.py:138:9:</a> PLW0101 Unreachable code in `run`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/file_server.py#L93'>tests/support/plugins/file_server.py:93:9:</a> PLW0101 Unreachable code in `__init__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/e2c747b5ad295424b08ff4fb03204129155666fc/blinksync/blinksync.py#L102'>blinksync/blinksync.py:102:5:</a> PLW0101 Unreachable code in `main`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/8574442c5709c429f01369c5ce969be8065c2f2b/libs/core/langchain_core/runnables/base.py#L5137'>libs/core/langchain_core/runnables/base.py:5137:13:</a> PLW0101 Unreachable code in `astream_events`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/3d40eb57259fbea100d5022f9ab04f5bb7686857/src/latch_cli/tinyrequests.py#L111'>src/latch_cli/tinyrequests.py:111:5:</a> PLW0101 Unreachable code in `_req`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/b65b4f5c6e46b2b930d48f4593032b86803e848a/examples/iterator/iterator.py#L64'>examples/iterator/iterator.py:64:9:</a> PLW0101 Unreachable code in `external_filter_func`
+ <a href='https://github.com/milvus-io/pymilvus/blob/b65b4f5c6e46b2b930d48f4593032b86803e848a/examples/iterator/iterator.py#L65'>examples/iterator/iterator.py:65:9:</a> PLW0101 Unreachable code in `external_filter_func`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/e5898b8d33ac943a60250e1466006c073a506e8c/pandas/_version.py#L111'>pandas/_version.py:111:5:</a> PLW0101 Unreachable code in `run_command`
+ <a href='https://github.com/pandas-dev/pandas/blob/e5898b8d33ac943a60250e1466006c073a506e8c/pandas/io/html.py#L1007'>pandas/io/html.py:1007:5:</a> PLW0101 Unreachable code in `_parse`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/c624060abdd57e365dcf35fb5aceafae422a0d06/rotkehlchen/chain/evm/decoding/morpho/utils.py#L64'>rotkehlchen/chain/evm/decoding/morpho/utils.py:64:5:</a> PLW0101 Unreachable code in `_query_morpho_vaults_api`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/18fdbc15196bbb8501435586ff60aab2c67aa345/zerver/actions/message_flags.py#L93'>zerver/actions/message_flags.py:93:5:</a> PLW0101 Unreachable code in `do_mark_all_as_read`
+ <a href='https://github.com/zulip/zulip/blob/18fdbc15196bbb8501435586ff60aab2c67aa345/zerver/views/streams.py#L1073'>zerver/views/streams.py:1073:5:</a> PLW0101 Unreachable code in `delete_in_topic`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/9a96d1acc4957742bfd7bdde6c059562e8e1d759/indico/util/decorators.py#L44'>indico/util/decorators.py:44:9:</a> PLW0101 Unreachable code in `__get__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/cd6eda4f84d3b52fcf9e79577188a5ee83f33eac/astropy/coordinates/name_resolve.py#L201'>astropy/coordinates/name_resolve.py:201:5:</a> PLW0101 Unreachable code in `get_icrs_coordinates`
+ <a href='https://github.com/astropy/astropy/blob/cd6eda4f84d3b52fcf9e79577188a5ee83f33eac/astropy/io/votable/tree.py#L3146'>astropy/io/votable/tree.py:3146:9:</a> PLW0101 Unreachable code in `_parse_binary`
+ <a href='https://github.com/astropy/astropy/blob/cd6eda4f84d3b52fcf9e79577188a5ee83f33eac/astropy/samp/tests/test_helpers.py#L37'>astropy/samp/tests/test_helpers.py:37:5:</a> PLW0101 Unreachable code in `assert_output`
+ <a href='https://github.com/astropy/astropy/blob/cd6eda4f84d3b52fcf9e79577188a5ee83f33eac/astropy/table/pprint.py#L137'>astropy/table/pprint.py:137:13:</a> PLW0101 Unreachable code in `_auto_format_func`
+ <a href='https://github.com/astropy/astropy/blob/cd6eda4f84d3b52fcf9e79577188a5ee83f33eac/astropy/utils/data.py#L1590'>astropy/utils/data.py:1590:5:</a> PLW0101 Unreachable code in `download_file`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0101 | 21 | 20 | 1 | 0 | 0 |

</p>
</details>




---

_Label `do-not-merge` added by @dylwil3 on 2025-04-21 19:33_

---

_Label `internal` added by @dylwil3 on 2025-04-21 19:33_

---

_Marked ready for review by @dylwil3 on 2025-04-21 19:37_

---

_Renamed from "[WIP] Cfg switch" to "Control flow: Switch statements" by @dylwil3 on 2025-04-21 19:38_

---

_Comment by @MichaReiser on 2025-04-22 16:22_

Oh, you work on your own fork. For stacked PRs, it's beneficial if you submit directly to this repo so that you can then set the base to a different branch.

---

_Comment by @MichaReiser on 2025-04-22 16:26_

The PR title is a bit confusing because Python doesn't have switch statements and I was surprised to see that this PR also implements `if` statements. I think it could have made sense to actually split those PRs into `if` statements and `match` statement because I can't think of a term that unambiously includes both. 

---

_Converted to draft by @dylwil3 on 2025-10-24 19:40_

---

_Closed by @dylwil3 on 2025-12-10 18:00_

---
